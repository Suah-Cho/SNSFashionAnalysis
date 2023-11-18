# SNSFashionAnalysis
SNS 패션 트렌드 분석 및 추천 서비스 프로젝트입니다👕

# 주제 정의
SNS 정보를 활용하여 트랜드 분석 후 사용자에게 패션 추천
- MZ세대에서 패션에 관심이 높아지면서 SNS(인스타그램)의 패션 데이터를 분석 후 사용자의 선호 패션 스타일에 따라 추천해주는 서비스 구현
- 트렌드 아이템/트렌드 색상 + 선호 스타일/퍼스널 컬러 => 맞춤형 코디 이미지 추천

# 데이터 수집
|데이터|설명|
|---|---|
|학습 데이터|SNS를 통한 데이터 약 40,000장<br>DeepFasion2 Dataset 약 10,000장|
|분석 데이터|인스타그램 수집 키워드 #ootd #오오티디 #fashion등을 통해 크롤링한 데이터 약 20,000장(인스타그램 올해의 해시태그 참조)|
|추천 데이터|각 스타일별 쇼핑몰피드 크롤링한 데이터 약 5,000장(casual, feminine, minimal, modern, street, vintage)|

# 모델 구현
## 1. Bianry Classification
#### Resnet18사용(CNN)
- 챌린징하지 않아 가벼우면서 모델 성능을 향상시킬 수 있는 모델
- SNS을 통한 약 40,000장의 인스타크롤링 데이터
- 패션 이미지와 패션 이미지가 아닌 것을 1과 0으로 구분된 CSV파일 포함
![image](https://github.com/Suah-Cho/SNSFashionAnalysis/assets/102336763/55106cfd-de68-4d88-8545-0ea8b64f1a01)
- 패션 이미지가 아닌 이미지 모두 제

## 2. Instance Segmentation
#### Matterport에서 제공하는 Mask R-CNN 사용
![image](https://github.com/Suah-Cho/SNSFashionAnalysis/assets/102336763/6e720a4b-c498-49e4-ab14-e74e339140af)
- 높은 정확도와 색을 추출해야하기 때문에 bounding box뿐만 아니라 해당 box 내 패션 아이템에 해당하는 pixel 위치정보를 포함하는 mask가 필요함

![image](https://github.com/Suah-Cho/SNSFashionAnalysis/assets/102336763/217add89-9ce0-43e5-8245-1ca89e0121fa)

![image](https://github.com/Suah-Cho/SNSFashionAnalysis/assets/102336763/a0000f00-ccd0-48ad-a5e0-53b80ee718eb)

#### DeepFasion2 Dataset
- 쇼핑몰 이미지와 소비자 리뷰 이미지 속 패션 아이템을 13개의 카테고리로 나눠 놓은 데이터
- 총 191,961 개의 train 데이터와 32,153개의 validation데이터, 62,629개의 test 데이터로 구성
- 각 이미지별 패션 아이템의 크기, 관점, 범주, 경계 박스, 픽셀별 마스크 등 13 종류의 정보를 담고 있는 annotation 파일도 포함

## 3. Color Extraction
- 이미지의 모델 픽셀 RGB값을 통해 배경(검정바탕, (0,0,0)) => 삭제
- KdTree를 이용하여 이미지의 RGB 픽셀과 가장 유사한 색 선택 -> 패션 아이템 색으로 설정

# 서비스
- Binary Classification -> Instance Segmentation 결과를 바탕으로 많이 사용된 아이템 및 색상을 트렌드 분석의 기준으로 정함.

![image](https://github.com/Suah-Cho/SNSFashionAnalysis/assets/102336763/08a30596-abeb-470a-92ae-d0d9d1aabff6)

# 기대 효과
#### 최신 패션 트렌드
최신 트렌드의 방향과 색상에 따른 패션 아이템 매칭 가능
#### 고객별 추천 시스텝
쇼핑몰에서 고객의 퍼스널 컬러와 기호에 따른 아이템 추천 가능
#### 다양한 추천의 가능성
더 많은 정보 데이터를 받으면 더욱 다양한 추천 가능
