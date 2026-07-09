<div align="center">
  <h1>☕ 서울시 상권 분석 및 매출 예측 파이프라인 구축</h1>
  <p><b>Prophet 및 SARIMAX 기반의 신촌 상권 커피-음료 업종 매출 예측 및 모델 고도화 프로토타입</b></p>
</div>

<br />

<h2> 1. 프로젝트 목적 및 As-Is 한계 개선</h2>
<p>
  본 프로젝트는 과거 프로젝트에서 수행했던 서울시 상권 분석 데이터셋을 바탕으로, 
  <b>'모델 선정의 타당성 확보'</b>와 <b>'정량적 오차율(MAPE) 검증 체계 구축'</b>을 위해 전면 재작업을 진행한 개인 고도화 세션입니다.
</p>

<ul>
  <li><b>기존 프로젝트(As-Is)의 한계:</b> 정량적 오차율 검증(MAPE) 누락, 소형 데이터셋에 부적합한 딥러닝(LSTM) 모델 선정</li>
  <li><b>개인 개선안(To-Be)의 방향:</b> 데이터 스케일에 최적화된 시계열 모델(Prophet, SARIMA) 도입, 오차율(MAPE) 검증 단계 추가</li>
</ul>

<hr />

<h2> 2. 핵심 모델별 실험 결과 및 서울시 행정동 검증</h2>

<table width="100%">
  <thead>
    <tr align="center" style="background-color: #f6f8fa;">
      <th>실험 모델</th>
      <th>외부 변수 반영</th>
      <th>검증 오차율 (MAPE)</th>
      <th>예측 결과 요약 및 비즈니스 평가</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center"><b>Prophet</b><br>(Best 🏆)</td>
      <td align="center">⭕<br><small>(is_corona)</small></td>
      <td align="center"><b>13.75%</b></td>
      <td>엔데믹 이후 신촌 상권의 우상향 회복세를 반영하여 반등하는 흐름을 예측함. 가장 오차율이 적게 나오며, 서울시 전 행정동 [커피-음료]매출 예측의 모델링으로 활용</td>
    </tr>
       <tr>
      <td align="center"><b>Prophet</b></td>
      <td align="center">❌<br></td>
      <td align="center"><b>41.55%</b></td>
      <td>코로나의 매출 하락세를 일반적인 트렌드로 오인하여 회복세를 예측하지 못함</td>
    </tr>
    <tr>
      <td align="center"><b>SARIMA</b></td>
      <td align="center">❌<br></td>
      <td align="center"><b>22.90%</b></td>
      <td>상권매출의 계절성을 반영했지만 코로나의 하락세를 일반적인 트렌드로 오인하여 회복세를 예측하지 못함</td>
    </tr>
        <tr>
      <td align="center"><b>SARIMAX</b></td>
      <td align="center">⭕<br><small>(is_corona)</small></td>
      <td align="center"><b>99.75%</b></td>
      <td>과거 코로나 충격폭(통계표상 약 71억 원 감산)을 강하게 학습하여 <b>과적합(Overfitting)</b> 발생. 엔데믹 회복기인 2022년 실제 매출 스케일을 반영하지 못하고 비현실적인 음수(-) 예측치를 도출하며 오차율  상승</td>
    </tr>
  </tbody>
</table>

<hr />
<h3>📊 2-1. [신촌동] Prophet 기반 2023년 미래 예측</h3>
<p>
  검증 오차율이 가장 낮았던 <b>Prophet(+외부 변수 반영)</b> 모델을 채택하여 2023년 미래 매출을 예측했습니다. 
  직전 연도(2022년)의 상권 회복 트렌드를 안정적으로 이어받아, 코로나 비수기를 지나 <b>점진적으로 반등 및 우상향하는 현실적인 비즈니스 흐름</b>을 성공적으로 포착해 냈습니다.
</p>
<div align="center">
  <img width="1078" height="377" alt="image" src="https://github.com/user-attachments/assets/4ecc61f7-ab93-4fff-812d-c074e3a1d15e" />
</div>

<br />

<h3>🌍 2-2. [서울시 전체 확장] 행정동별 모델링 예측 안정성 분류</h3>
<p>
  신촌동 프로토타입 파이프라인을 <b>함수화(Function)하여 서울시 전체 376개 행정동 코드로 예측</b>을 실행한 결과입니다. 
  상권별 변동성 크기에 따라 예측 성과를 3가지 그룹으로 분류하여 모델의 신뢰도를 확보했습니다.
</p>

<table width="80%" align="center">
  <thead>
    <tr align="center" style="background-color: #f6f8fa;">
      <th>분류 유형</th>
      <th>행정동 개수 (개)</th>
      <th>비율 (%)</th>
      <th>그룹별 평균 오차율 (MAPE)</th>
    </tr>
  </thead>
  <tbody>
    <tr align="center">
      <td><b>🟢 안정 (예측 매우 잘됨)</b></td>
      <td>34</td>
      <td>9.04%</td>
      <td><b>6.88%</b></td>
    </tr>
    <tr align="center">
      <td><b>🟡 주의 (변동성 중간)</b></td>
      <td>112</td>
      <td>29.79%</td>
      <td><b>17.32%</b></td>
    </tr>
    <tr align="center">
      <td><b>🔴 경고 (변동성 큼)</b></td>
      <td>230</td>
      <td>61.17%</td>
      <td><b>85.49%</b></td>
    </tr>
  </tbody>
</table>

<br />
<div align="center">
  <img width="307" height="329" alt="image" src="https://github.com/user-attachments/assets/954f35c9-a7cd-4c6e-a0ae-8bc595e64123" />
</div>
<br />

<p>
  📌 <b>확장 분석 시사점:</b> 서울시 전체 상권의 약 40%에 달하는 상권(안정+주의)에서는 Prophet 모델이 20% 이하의 예측력을 보여주었습니다. 
  다만, 변동성이 지나치게 큰 대다수의 상권(61.17%)에서는 오차율이 치솟는 한계가 관찰되어, 상권의 특성을 반영하는 외부 변수가 필요하다 판단됩니다.
</p>
