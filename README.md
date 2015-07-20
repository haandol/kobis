
#  Pandas 에서 csv 를 읽어들여 DataFrame으로 만들기


    # coding: utf-8
    
    import pandas as pd


    df = pd.read_csv('kobis.csv'); df[:3]




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>lang</th>
      <th>genre</th>
      <th>title</th>
      <th>actor</th>
      <th>director</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>32544</td>
      <td>ko</td>
      <td>범죄,서부극(웨스턴)</td>
      <td>옥스보우 인서던트</td>
      <td>헨리 폰다,다나 앤드류스,메리 배스 휴즈,앤소니 퀸</td>
      <td>윌리엄 A. 웰만</td>
    </tr>
    <tr>
      <th>1</th>
      <td>48171</td>
      <td>ko</td>
      <td>판타지,드라마</td>
      <td>불을 찾아서</td>
      <td>에버렛 맥길,론 펄먼,래 돈 총</td>
      <td>장 자끄 아노</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34171</td>
      <td>ko</td>
      <td>드라마,멜로/로맨스</td>
      <td>비개인 오후를 좋아하세요</td>
      <td>최수종,이미연,홍학표,김은숙</td>
      <td>김우봉</td>
    </tr>
  </tbody>
</table>
</div>



# DataFrame 객체의 행과 열에 접근하기


    print df['title'][:3]
    print
    print df.ix[0]

    0        옥스보우 인서던트
    1           불을 찾아서
    2    비개인 오후를 좋아하세요
    Name: title, dtype: object
    
    id                                 32544
    lang                                  ko
    genre                        범죄,서부극(웨스턴)
    title                          옥스보우 인서던트
    actor       헨리 폰다,다나 앤드류스,메리 배스 휴즈,앤소니 퀸
    director                       윌리엄 A. 웰만
    Name: 0, dtype: object


# reindex 를 이용하여 데이터 인덱스 와 열 변경하기


    df2 = df.reindex(index=df.id, columns=['genre', 'title', 'actor', 'director']); df2[:3]




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>genre</th>
      <th>title</th>
      <th>actor</th>
      <th>director</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>32544</th>
      <td>드라마</td>
      <td>길의노래</td>
      <td>NaN</td>
      <td>사티야지트 레이</td>
    </tr>
    <tr>
      <th>48171</th>
      <td>액션,코미디,범죄</td>
      <td>브루스 브라더스 2000</td>
      <td>댄 애크로이드,존 굿맨</td>
      <td>존 랜디스</td>
    </tr>
    <tr>
      <th>34171</th>
      <td>NaN</td>
      <td>극진</td>
      <td>NaN</td>
      <td>정연두</td>
    </tr>
  </tbody>
</table>
</div>



# dropna 로 값이 없는 열을 포함한 행들을 제거


    df2.dropna(how='any')[:3]




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>genre</th>
      <th>title</th>
      <th>actor</th>
      <th>director</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>48171</th>
      <td>액션,코미디,범죄</td>
      <td>브루스 브라더스 2000</td>
      <td>댄 애크로이드,존 굿맨</td>
      <td>존 랜디스</td>
    </tr>
    <tr>
      <th>1941</th>
      <td>성인물(에로)</td>
      <td>만원전철 속 그녀2</td>
      <td>니시노 쇼</td>
      <td>칸바라 이구토</td>
    </tr>
    <tr>
      <th>42169</th>
      <td>드라마</td>
      <td>미.유.뎀</td>
      <td>리마 두아르트,스테니오 가르시아,레지나 케이스</td>
      <td>앤드류차 웨딩턴</td>
    </tr>
  </tbody>
</table>
</div>



# ‘박찬욱’ 감독의 영화제목만 가져오기


    gb = df2.groupby('director')
    print gb
    print gb.get_group('박찬욱').title

    <pandas.core.groupby.DataFrameGroupBy object at 0x113f42e50>
    id
    42351             파란만장
    38863             올드보이
    34486               심판
    47448    A Rose Reborn
    34494      쓰리, 몬스터 '컷'
    276             미쓰 홍당무
    Name: title, dtype: object



    print (df2.director == '박찬욱')[:3]
    print
    print df2[df2.director == '박찬욱'].title

    id
    32544    False
    48171    False
    34171    False
    Name: director, dtype: bool
    
    id
    42351             파란만장
    38863             올드보이
    34486               심판
    47448    A Rose Reborn
    34494      쓰리, 몬스터 '컷'
    276             미쓰 홍당무
    Name: title, dtype: object



    df2[:5]




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>genre</th>
      <th>title</th>
      <th>actor</th>
      <th>director</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>32544</th>
      <td>드라마</td>
      <td>길의노래</td>
      <td>NaN</td>
      <td>사티야지트 레이</td>
    </tr>
    <tr>
      <th>48171</th>
      <td>액션,코미디,범죄</td>
      <td>브루스 브라더스 2000</td>
      <td>댄 애크로이드,존 굿맨</td>
      <td>존 랜디스</td>
    </tr>
    <tr>
      <th>34171</th>
      <td>NaN</td>
      <td>극진</td>
      <td>NaN</td>
      <td>정연두</td>
    </tr>
    <tr>
      <th>49699</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>33786</th>
      <td>애니메이션</td>
      <td>프리스키즈 어드벤처랜드</td>
      <td>NaN</td>
      <td>야네스 헨드릭즈</td>
    </tr>
  </tbody>
</table>
</div>



# Word2Vec 에 학습시키기 위해 데이터 형태 변환


    df3 = df.reindex(columns=['genre', 'title', 'director', 'actor']).dropna(how='any').sort('director'); df3[:5]




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>genre</th>
      <th>title</th>
      <th>director</th>
      <th>actor</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1963</th>
      <td>멜로/로맨스,드라마</td>
      <td>거짓말 섹스가 좋아2</td>
      <td>3G 같은 인생</td>
      <td>강수지,윤상두</td>
    </tr>
    <tr>
      <th>41491</th>
      <td>범죄,드라마</td>
      <td>왓 앨리스 파운드</td>
      <td>A. 딘 벨</td>
      <td>주디스 아이비,빌 레이몬드</td>
    </tr>
    <tr>
      <th>30377</th>
      <td>드라마,판타지,미스터리</td>
      <td>디아더 사이드 오브 더 트랙스</td>
      <td>A.D. 칼보</td>
      <td>채드 린드버그,브렌단 페르</td>
    </tr>
    <tr>
      <th>30286</th>
      <td>스릴러,SF</td>
      <td>아마겟돈: 카운트다운</td>
      <td>A.F. 실버</td>
      <td>킴 리틀,마크 헹스트</td>
    </tr>
    <tr>
      <th>1068</th>
      <td>액션,드라마,미스터리,멜로/로맨스,스릴러</td>
      <td>가지니</td>
      <td>A.R. 무루가도스</td>
      <td>아미르 칸,아신</td>
    </tr>
  </tbody>
</table>
</div>




    for i in df3[:5].index:
        data = df3.ix[i]
        print data.title, ' '.join(data.director.replace(' ', '').split(',')), ' '.join(data.actor.split(','))

    거짓말 섹스가 좋아2 3G같은인생 강수지 윤상두
    왓 앨리스 파운드 A.딘벨 주디스 아이비 빌 레이몬드
    디아더 사이드 오브 더 트랙스 A.D.칼보 채드 린드버그 브렌단 페르
    아마겟돈: 카운트다운 A.F.실버 킴 리틀 마크 헹스트
    가지니 A.R.무루가도스 아미르 칸 아신


# gensim 을 통해 Word2vec 를 임포팅 하고 문장을 학습시키기


    from gensim.models import word2vec
    import logging
    logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s', level=logging.INFO)


    with open('sentences.txt', 'w') as fp:
        for value in df3.values:
            genre = value[0].replace(',', ' ')
            title = value[1].replace(' ', '')
            director = value[2].replace(' ', '')
            for actor in value[3].split(','):
                fp.write('%s %s %s %s\n' % (genre, title, director, actor.replace(' ', '')))


    sentences = word2vec.Text8Corpus('sentences.txt')


    model = word2vec.Word2Vec(sentences, size=100, window=5, min_count=5, workers=4)

# Word2vec 학습된 모델에 질의 보내기


    result = model.most_similar(positive=[u'올드보이', u'박찬욱'], negative=[u'스릴러'], topn=1)


    for el in result:
        print el[0]

    미쓰홍당무



    result = model.most_similar(positive=[u'다크나이트라이즈', u'크리스토퍼놀란'], negative=[u'히스레저'], topn=1)


    for el in result:
        print el[0]

    다크나이트

