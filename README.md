# aiffel_recommendation
모두의 연구소 데이터 사이언티스트 '딥러닝 기반 추천 시스템' 프로젝트 제출 레포지토리입니다.  

## 폴더 구조
```
📁 aiffel_recommendation/  
│
├── 📁 autoint/                                   # autoint 모델 테스트 & 배포   
│   └── 📁 __pycache__/                           # 파이썬 캐시 파일_무시  
|   └── 📁 data/  
│       ├── 📁 ml-1m                              # 각 데이터별 1차 전처리 csv 파일  
│       ├── 📄 autoIntMLP_field_dims.npy          # + 모델 적용 field_dims  
│       ├── 📄 autoIntMLP_label_encodes.pkl       # + 모델 적용 label_encoder  
│       ├── 📄 field_dims.npy  
│       ├── 📄 movielens_rcmm_v1.csv              # 랜덤 샘플링 기반 전처리 데이터  
│       └── 📄 movielens_rcmm_v2.csv              # 실제 적용된 사용자 선호도 기반 전처리  
│  
|   └── 📁 model/  
│       ├── 📄 autoIntMLP_model_weights.h5        # AutoInt+ 모델 가중치   
│       └── 📄 autoInt_model_weights.h5  
│  
|   └── 📄 autoint.py  
|   └── 📄 autointplus.py                         # AutoInt+ 모델 정의   
|   └── 📄 model_load_test.ipynb  
|   └── 📄 plus_model_train.ipynb                 # AutoInt+ 모델 성능 테스트 노트북  
|   └── 📄 show_st.py  
|   └── 📄 show_st_plus.py                        # AutoInt+ 모델 streamlit 배포 파일  
│  
├── 📄 0711_9.나만의 딥러닝 기반 추천 시스템 구축하기.ipynb # EDA & AutoInt 모델 구현                 
│  
├── 📄 requirements.txt            # Python 패키지  
│  
└── 📄 README.md  
```
  
## 실험 내용
AutoInt+ 모델의 성능을 개선하고자 다양한 하이퍼파라미터를 조합하여 실험을 진행하였다.  
실험은 autoint/plus_model_train.ipynb 파일에서 수행되었으며, 실험 결과 베스트 모델을 선정하여 Streamlit으로 배포하였다.  
  
### 실험 설정
다음과 같은 하이퍼파라미터 조합을 대상으로 실험을 진행:
- epochs: 3 또는 5
- learning_rate: 0.0001 / 0.0005 / 0.001
- dropout: 0.3 ~ 0.7
- batch_size: 1024 또는 2048
- embed_dim: 16 또는 32
- att_head_num: 2 / 4 / 8 ← 기본 어텐션 헤드 수는 2개이나 추가로 설정하여 테스트 해보기 위함으로 추가  
  
![model-parameter](https://github.com/user-attachments/assets/da34c7b1-3fbe-495c-95d7-efc4caa3f06d)
  
### 실험 결과 요약
![model-test-result](https://github.com/user-attachments/assets/6f1f357d-6d14-4544-8150-b781a2e49c46)  
  
### 실험 결과 분석
- **어텐션 헤드 수(att_head_num)** 는 2, 4, 8로 설정해가며 비교했으며, 일부 실험에서는 성능이 향상되었으나  
Exp 1, 2, 3을 확인하면 동일한 조건에서 헤드 수를 늘린다고 해서 모델의 성능이 꼭 높아지는 것은 아니었다. 

- 성능이 가장 좋았던 Exp_5 모델의 조건을 바탕으로,
embed_dim을 32로 높여 추가 실험(Exp_6)도 진행해보았더니 성능이 미약하지만 향상되어 best_model이 되었다. 

- 전체적으로 학습률 0.001, 드롭아웃 0.5, 배치 사이즈 1024에서 안정적이고 좋은 성능을 보였다.
- 헤드 수가 많을수록 학습 시간이 오래 걸린다. 
  
## Streamlit
https://aiffelrecommendation-movielens.streamlit.app/  
(* Streamlit Cloud의 메모리 문제로 인하여 페이지가 계속 죽는 문제가 발생한다..)  
  
**로컬 환경 테스트**  
  
![local-streamlit](https://github.com/user-attachments/assets/2073627f-4c54-409b-b797-04d5b023cea9)  
