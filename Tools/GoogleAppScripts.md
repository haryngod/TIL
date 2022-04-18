# App Scripts

## 영어단어 생성기

> 사내의 영어 스터디를 만들었다.
먼저 단어 공부를 해보자는 의견이 나와서 모두가 가지고 있는 토익 단어장 1day 40개 정도를 월~목요일에 외우고 주 2회 시험을 치고있다. 당번을 정해 한사람이 액셀에 2일치 단어와 뜻을 적어 카카오톡으로 보내주고, 시험지와 답지를 만들어온다. 나는 좀 더 효율적이게 매 회차의 단어장, 시험지, 답지를 생성 할 방법을 찾고 싶었다. 그래서 구글의 App Scripts를 사용해 만들어 보았다.

### 요구사항
* 책의 모든 단어와 뜻이 있는 시트 필요
* 해당 회차의 시작일을 입력 받음
* 2일치 단어 약 80개가 순서를 가지고 [단어, 뜻, 단어, 뜻]과 같은 형태로 정렬
* 해당 회차 단어들이 랜덤으로 섞여 [단어, 빈칸, 단어, 빈칸]과 같은 형태로 생성
* 해당 회차 단어들이 시험지와 같은 순서로 정렬 되어 [단어, 뜻, 단어, 뜻] 형태로 생성

### 절차
* 먼저 책에 나오는 단어들이 [일, 단어, 뜻] 형태가 되게 작성하였고 해당 시트 이름은 [단어장]
* 그리기를 통해 버튼이 될 도형을 만들어 준다
* 확장 프로그램 > App Script를 통해 스크립트 작성 > [스크립트 링크](https://script.google.com/u/0/home/projects/1cFdu-v1N0IGFkU532cYedP81u6Gmtz9MlX7CfQlf8c0jmy5Xtt22xH58/edit)
* 시트가 잘 동작한다 :) > [시트 링크](https://docs.google.com/spreadsheets/d/1pD-nfo6186tNMV0hmT5HNqDnxtumCWkS9vAoM6tezl4/edit?usp=sharing)

### 느낀점
* 에전부터 액셀의 매크로가 뭔지 궁금하고 해보고 싶었는데, 귀차니즘 때문에 도전해보지 못했다.
* 액셀보다는 구글 시트를 많이 사용하는지라 한번 해볼까 하고 주말에 잠깐 시간을 내서 해보았는데 동작하는 것을 보니 재밌었다.
* html과 script를 섞으면 입력창이 떠서 입력값을 입력하고 동작하는걸 처음에 기획했는데, 잘 되지 않았다. 다음에는 그런 방식으로도 작성해보고 싶다.

### 스크립트
```python
function createTestBtn() {
 const sheet = SpreadsheetApp.getActiveSpreadsheet();
  const vocaSheet = sheet.getSheetByName('단어장');
  const todayVocaSheet = sheet.getSheetByName('이번회차단어');
  const testSheet = sheet.getSheetByName('시험지');
  const answerSheet = sheet.getSheetByName('답지');

  var startDay = vocaSheet.getRange("D2").getValue();
  var endDay = startDay+1;
  
  var data = vocaSheet.getDataRange().getValues();
  data = data.slice(1, data.length);

  var vocaList = new Array;
  for( i in data ){
    if (data[i][0] < startDay) continue; 
    else if( data[i][0] > endDay ) break;
    else
      vocaList.push([data[i][1], data[i][2]]);
  }

  setSheetForm(todayVocaSheet);
  setSheetForm(testSheet);
  setSheetForm(answerSheet);

  var firstList = vocaList.slice(0, vocaList.length/2);
  var secondList = vocaList.slice(vocaList.length/2, vocaList.length );

  fillSheet(todayVocaSheet, firstList, secondList, false);

  shuffle(vocaList);  

  firstList = []; secondList = [];

  firstList = vocaList.slice(0, vocaList.length/2);
  secondList = vocaList.slice(vocaList.length/2, vocaList.length );

  
  fillSheet(testSheet, firstList, secondList, true);
  fillSheet(answerSheet, firstList, secondList, false);
}

function fillSheet(ss, fl, sl, test)
{
  ss.getRange(2, 1, fl.length, fl[0].length).setValues(fl);
  ss.getRange(2, fl[0].length+1, sl.length, sl[0].length).setValues(sl);

  if(test)
  {
    ss.getRange(2, 2, fl.length, 1).clearContent();
    ss.getRange(2, 4, fl.length, 1).clearContent();
  }
}

function setSheetForm(ss)
{
  ss.clear();
  
  ss.getRange("A1").setValue("단어");
  ss.getRange("C1").setValue("단어");
  ss.getRange("B1").setValue("뜻");
  ss.getRange("D1").setValue("뜻");

  var colorList = ["#9999FF", "#FFCC00", "#FF9999", "#CCCC00", "#66CC99"];
  ss.getRange(1, 1, 1, 4).setBackground(colorList[randomNum(0, colorList.length-1)]);
}

function randomNum(min, max){
    var randNum = Math.floor(Math.random()*(max-min+1)) + min;
    return randNum;
}

function shuffle(array) { 
  array.sort(() => Math.random() - 0.5); 
}
```