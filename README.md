# P1-Image-Classification

# 전체 개요 설명
COVID-19의 확산으로 우리나라는 물론 전 세계 사람들은 경제적, 생산적인 활동에 많은 제약을 가지게 되었습니다. 우리나라는 COVID-19 확산 방지를 위해 사회적 거리 두기를 단계적으로 시행하는 등의 많은 노력을 하고 있습니다. 과거 높은 사망률을 가진 사스(SARS)나 에볼라(Ebola)와는 달리 COVID-19의 치사율은 오히려 비교적 낮은 편에 속합니다. 그럼에도 불구하고, 이렇게 오랜 기간 동안 우리를 괴롭히고 있는 근본적인 이유는 바로 COVID-19의 강력한 전염력 때문입니다.
감염자의 입, 호흡기로부터 나오는 비말, 침 등으로 인해 다른 사람에게 쉽게 전파가 될 수 있기 때문에 감염 확산 방지를 위해 무엇보다 중요한 것은 모든 사람이 마스크로 코와 입을 가려서 혹시 모를 감염자로부터의 전파 경로를 원천 차단하는 것입니다. 이를 위해 공공 장소에 있는 사람들은 반드시 마스크를 착용해야 할 필요가 있으며, 무엇 보다도 코와 입을 완전히 가릴 수 있도록 올바르게 착용하는 것이 중요합니다. 하지만 넓은 공공장소에서 모든 사람들의 올바른 마스크 착용 상태를 검사하기 위해서는 추가적인 인적자원이 필요할 것입니다.
따라서, 우리는 카메라로 비춰진 사람 얼굴 이미지 만으로 이 사람이 마스크를 쓰고 있는지, 쓰지 않았는지, 정확히 쓴 것이 맞는지 자동으로 가려낼 수 있는 시스템이 필요합니다. 이 시스템이 공공장소 입구에 갖춰져 있다면 적은 인적자원으로도 충분히 검사가 가능할 것입니다.

# 평가 방법
- Submission 파일에 대한 평가는 F1 Score 를 통해 진행합니다
- F1 Score 에 대해 간략히 설명 드리면 다음과 같습니다.
![234e660e-d340-4623-9628-932f5ef8b9bb](https://user-images.githubusercontent.com/55614265/113950298-ebbe0480-984b-11eb-8ca7-933738c97589.png)

# 데이터 개요
마스크를 착용하는 건 COIVD-19의 확산을 방지하는데 중요한 역할을 합니다. 제공되는 이 데이터셋은 사람이 마스크를 착용하였는지 판별하는 모델을 학습할 수 있게 해줍니다. 모든 데이터셋은 아시아인 남녀로 구성되어 있고 나이는 20대부터 70대까지 다양하게 분포하고 있습니다. 간략한 통계는 다음과 같습니다.
- 전체 사람 명 수 : 4,500
- 한 사람당 사진의 개수: 7 [마스크 착용 5장, 이상하게 착용(코스크, 턱스크) 1장, 미착용 1장]
- 이미지 크기: (384, 512)
학습 데이터와 평가 데이터를 구분하기 위해 임의로 섞어서 분할하였습니다. 60%의 사람들은 학습 데이터셋으로 활용되고, 20%는 public 테스트셋, 그리고 20%는 private 테스트셋으로 사용됩니다
진행중인 대회의 리더보드 점수는 public 테스트셋으로 계산이 됩니다. 그리고 마지막 순위는 private 테스트셋을 통해 산출한 점수로 확정됩니다. private 테스트셋의 점수는 대회가 진행되는 동안 볼 수 없습니다.
입력값. 마스크 착용 사진, 미착용 사진, 혹은 이상하게 착용한 사진(코스크, 턱스크)
결과값. 총 18개의 class를 예측해야합니다. 결과값으로 0~17에 해당되는 숫자가 각 이미지 당 하나씩 나와야합니다.
- Class Description: 마스크 착용여부, 성별, 나이를 기준으로 총 18개의 클래스가 있습니다.
![56bd7d05-4eb8-4e3e-884d-18bd74dc4864](https://user-images.githubusercontent.com/55614265/113950407-3475bd80-984c-11eb-9056-ef149ce5a28e.png)

# 코드 설명
```
$> tree -d
.
├── data pre-processing.ipynb : 기본적으로 ImageFolder 사용을 위한 전처리와 추가적으로 age filtering과 k-fold cross-validation 그리고 pseudo labeling을 위한 데이터 전처리 코드
├── transfer_learning.ipynb : 전이학습을 위한 코드
└── inference.ipynb : inference와 ensemble 그리고 submission.csv 생성을 위한 코드
```

# Wrap up Report

## 기술적인 도전

### 본인의 점수 및 순위

- LB 점수 accuracy: 81.1746%, f1: 0.7648, 9등
<img width="980" alt="스크린샷 2021-04-09 10 41 21" src="https://user-images.githubusercontent.com/55614265/114117671-2abe8980-9922-11eb-83bb-cb01f75a75aa.png">

### 검증(Validation)전략

1. 제공된 dataset에 age filtering(>= 58) 이후 10-fold cross-validation을 적용했습니다.
2. 그리고 train dataset에 pseudo labeling 된 eval dataset을 추가했습니다.

### 사용한 모델 아키텍처 및 하이퍼 파라미터

- 아키텍쳐: ResNet34
    1. LB 점수 accuracy : 81.5238%, f1 : 0.7669%
    2. transforms.CenterCrop(384)
    3. transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
    4. batch_size=4
    5. LabelSmoothingLoss()
    6. optim.SGD(model_ft.parameters(), lr=0.0001, momentum=0.9)
    7. lr_scheduler.StepLR(optimizer_ft, step_size=10, gamma=0.9)
    8. num_epochs=15
    9. 해당 competition에서는 model보다는 data 자체가 중요했다고 생각합니다.
    10. 따라서, 최소한의 성능이 보장되는 model만 구현하고 data에 집중했습니다.

### 앙상블 방법

1. Hard Voting
2. Soft Voting

### 시도했으나 잘 되지 않았던 것들

1. 해당 dataset에서는 Data Augmentation 했던 것이 생각보다 성공적이지 않아서 아쉬웠습니다.
    1. data가 워낙 깔끔했기 때문에 이러한 결과가 나온 것으로 생각됩니다.
    2. 따라서, CenterCrop과 Normalize 정도만 사용했습니다.
2. 개인적으로 시도했을 때, 3X2X3-class classifier multi model로 했던 것이 생각보다 성공적이지 않아서 아쉬웠습니다.
    1. 한두 번의 시도로 너무 성급하게 포기한 것 같아 아쉽습니다.
    2. 결과론적으로 학습 시간에 이점이 있었던 18-class classifier single model이 해당 competition에는 적합했던 것 같습니다.
3. FocalLoss와 F1Loss가 생각보다 성공적이지 않아서 아쉬웠습니다.
4. 외부 데이터 추가
    1. Flickr-Faces-HQ (FFHQ), Correctly Masked Face Dataset (CMFD), Incorrectly Masked Face Dataset (IMFD) 그리고 All-Age-Faces-Dataset을 추가했습니다.
    2. 과도한 일반화로 인해 실패했다고 생각됩니다.
    3. 해당 competition에서는 일반화보다는 train과 eval dataset에 overfitting 되는 것이 중요했다고 생각합니다.
5. 이미지 분류 문제가 아니라, 이미지 매칭 문제로 해결하고자 했습니다.
    1. eval dataset에는 1,800명이 있고 한 사람당 7장의 사진을 가지고 있습니다.
    2. 그리고 마스크를 착용한 사람의 나이를 정확하게 분류하지 못하는 것이 성능 저하의 원인이라고 생각했습니다.
    3. 따라서 동일한 사람의 사진을 7장 매칭 후, 미착용 사진으로 성별과 나이를 분류하고 이를 바탕으로 나머지 6장의 사진을 분류한다면 성능이 오를 것으로 생각했습니다.
    4. 물론, 일반적인 방법은 아니며, eval dataset의 허점을 이용하고자 한 것입니다.
    5. 이미지 매칭을 위해서 단순 유사도, ImageHash, deepface 등 다양한 방법들을 사용했습니다.
    6. 그 결과, 유사한 데이터가 많아 동일한 사람의 사진을 7장 매칭하는 것은 불가능에 가깝다고 생각했습니다.
    7. 이미지 매칭에서 생기는 성능 저하나 이미지 분류에서 생기는 성능 저하가 큰 차이가 없을 것으로 생각되어, 이후에는 이미지 분류에 집중했습니다.

## 학습과정에서의 교훈

### 학습과 관련하여 개인과 동료로서 얻은 교훈

1. U Stage에서 같은 조였던 서일님에게 많은 도움을 받았습니다.
    1. 본인이 가지고 있는 정보를 아낌없이 공유해주었습니다. (위에서 언급한 외부 데이터도 공유해주셨습니다.)
    2. Competition의 특성상 핵심이 되는 정보는 공유하지 않았는데, 서일님에게는 모두 공유를 했고 추가로 조언도 받을 수 있었습니다.
    3. 모든 사람과는 아니더라도, 마음이 맞는 사람들과 정보를 공유하는 것도 괜찮은 것 같다고 생각했습니다.
2. 예전에 pseudo labeling을 좋은 방법이라고 생각하지는 않았는데, 이번 대회를 통해서 Semi-Supervised Learning인 pseudo labeling의 힘을 깨달을 수 있었습니다.

### 피어세션을 진행하며 좋았던 부분과 동료로부터 배운 부분

1.	대회 초반, 다른 사람들의 다양한 시도와 결과를 참고할 수 있어서 좋았습니다.
2.	랜덤으로 진행된 피어세션 덕분에, 커뮤니케이션의 중요성을 느낄 수 있었습니다.

## 마주한 한계와 도전숙제

### 아쉬웠던 점들

- 성능이 좋았던 단 하나의 모델에만 집중하여 아쉬웠습니다.
    1. 하나의 모델로만 진행하다 보니, 유사한 모델과 결과만 생성되어 앙상블에서 큰 효과를 보지 못했습니다.
    2. 그리고 pseudo labeling으로 특정 성능에 수렴했을 때, 벗어날 방법이 없었습니다.

### 한계/교훈을 바탕으로 다음 스테이지에서 새롭게 시도해볼 것

- 다음 스테이지는 한국어 언어 모델 학습 및 다중 과제 튜닝 (KLUE) 입니다. 어떤 task로 competition을 하는지 모르기 때문에, 어떤 시도를 해볼지 아직 잘 모르겠습니다. 만약 pretrain된 언어 모델을 사용한다면, 적어도 2개 이상의 언어 모델을 가지고 competition을 진행해볼 계획입니다.
