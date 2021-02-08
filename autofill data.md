CONTRACTOR_ID = 1
# 血圧
GENERIC1_ID = 1
# 睡眠時間
GENERIC2_ID = 2
# 一日平均歩数
GENERIC3_ID = 3
# 一日平均飲酒量
GENERIC4_ID = 4
# 喫煙歴
GENERIC5_ID = 5
# 有
CHOICE1_ID = 1
# 無
CHOICE2_ID = 2

 

from random import choice, random
from datetime import date

 

from django.utils import timezone
from django.db import transaction

 

from cbb_models.models.user import TExamUser, TExamUserGenericItem
from cbb_models.models.contractor import TContractor, TGenericItem
from cbb_models.models.cogstate import TCogstate, TCogstateWorkflowInstance, TCogstateVisitSession
from cbb_api.util.exam_cogstate_common import workflow_result_synchro

 

year30_enum = [i for i in range(1980, 1990)] * 1
year40_enum = [i for i in range(1970, 1980)] * 2
year50_enum = [i for i in range(1960, 1970)] * 3
year60_enum = [i for i in range(1950, 1960)] * 3
year70_enum = [i for i in range(1940, 1950)] * 2
year80_enum = [i for i in range(1930, 1940)] * 1
year_enum = year30_enum + year40_enum + year50_enum + year60_enum + year70_enum + year80_enum
generic1_salt = [i for i in range(-30, 31)]
generic2_salt = [i for i in range(-2, 3)]
month_enum = [i for i in range(1, 13)]
sex_enum = ['F', 'M']
young_rank_enum = ['A', 'A', 'A', 'A', 'B']
middle_rank_enum = ['A', 'A', 'B', 'B', 'C']
old_rank_enum = ['A', 'B', 'B', 'C', 'C']
generic_enum = [GENERIC1_ID, GENERIC2_ID, GENERIC3_ID, GENERIC4_ID, GENERIC5_ID]
a_rank_score = [i / 10 for i in range(200, 300)] + \
               [i / 10 for i in range(300, 401)] * 2 + \
               [i / 10 for i in range(400, 420)]
b_rank_score = [i / 10 for i in range(150, 200)]
c_rank_score = [i / 10 for i in range(80, 150)]
score_delta = [i for i in range(-8, 9)]
brain_age_diff = [i / 10 for i in range(20, 50)]
generic1_scale = [80, 170]
generic2_scale = [6, 12]

 

timezone_now = timezone.now()
contractor = TContractor.objects.get(id=CONTRACTOR_ID)


 


def drinking(score):
    score = float(score)
    block = 50 - int(score)
    if block == 0:
        block = 1
    return choice([str(i) for i in range(100 * (block - 1), 100 * block)])

 


def walking(score):
    score = float(score)
    block = int(score)
    if block == 0:
        block = 1
    return choice([str(i) for i in range(400 * (block - 1), 400 * block)])

 


def smoking(score):
    score = float(score)
    if score > 40:
        return CHOICE2_ID
    if score > 20:
        return choice([CHOICE1_ID, CHOICE2_ID, CHOICE2_ID])
    if score > 15:
        return choice([CHOICE1_ID, CHOICE1_ID, CHOICE2_ID])
    if score > 10:
        return choice([CHOICE1_ID, CHOICE1_ID, CHOICE1_ID, CHOICE2_ID])
    return CHOICE1_ID

 


def square_curve_compute(scale, score, need_float=False):
    middle = (scale[0] + scale[1]) * 0.5
    half = middle - scale[0]
    diff = 50.0 - float(score)
    delta = (diff ** 0.5 / (50 ** 0.5)) * half
    salt = choice(generic2_salt) if need_float else choice(generic1_salt)
    plus = middle + delta + salt * random()
    minus = middle - delta + salt * random()
    if need_float:
        return choice(['%.1f' % plus, '%.1f' % minus])
    else:
        return choice(['%.0f' % plus, '%.0f' % minus])

 


def get_random_result(old, rank_enum):
    rank = choice(rank_enum)
    if rank == 'A':
        score = choice(a_rank_score)
        brain_age = '%.1f' % (old - choice(brain_age_diff))
    elif rank == 'B':
        score = choice(b_rank_score)
        brain_age = '%.1f' % float(old)
    else:
        score = choice(c_rank_score)
        brain_age = '%.1f' % (old + choice(brain_age_diff))
    score += choice(score_delta)
    return rank, '%.1f' % score, brain_age

 


with transaction.atomic():
    for i in range(1, 101):
        print(i)
        unique_identifier = 'test202010%05d' % i
        year = choice(year_enum)
        timezone_year = timezone_now.year
        old = timezone_year - year
        if old < 40:
            rank, score, brain_age = get_random_result(old, young_rank_enum)
        elif old < 60:
            rank, score, brain_age = get_random_result(old, middle_rank_enum)
        else:
            rank, score, brain_age = get_random_result(old, old_rank_enum)
        birthday = date(year=year, month=choice(month_enum), day=1)
        sex = choice(sex_enum)
        exam_user = TExamUser(
            identification_id=unique_identifier,
            birthday=birthday,
            sex=sex,
            contractor=contractor
        )
        exam_user.save()
        for generic_id in generic_enum:
            if generic_id == GENERIC1_ID:
                input_value = square_curve_compute(generic1_scale, score)
            elif generic_id == GENERIC2_ID:
                input_value = square_curve_compute(generic2_scale, score, True)
            elif generic_id == GENERIC3_ID:
                input_value = walking(score)
            elif generic_id == GENERIC4_ID:
                input_value = drinking(score)
            else:
                input_value = smoking(score)
            generic_item = TGenericItem.objects.get(id=generic_id)
            exam_generic = TExamUserGenericItem(
                exam_user=exam_user,
                generic_item=generic_item,
                input_value=input_value
            )
            exam_generic.save()
        cog = TCogstate(
            exam_user=exam_user,
            is_completed=True,
            info_date=timezone_now,

        )
        cog.save()
        workflow = TCogstateWorkflowInstance(
            cogstate=cog,
            workflow_auth_token=unique_identifier,
            workflow_instance_id=unique_identifier,
            score_rank=rank,
            score_score=score,
            score_brain_age=brain_age,
            locality_code='ja_JP',
            test_identifier=unique_identifier,
            url=f'http://{unique_identifier}.com',
            user_agent=unique_identifier,
            status='Complete',
            exam_end_datetime=timezone_now
        )
        workflow.save()
        workflow_result_synchro(workflow, auto_save=True)