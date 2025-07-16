<img src="https://companieslogo.com/img/orig/MDB_BIG-ad812c6c.png?t=1648915248" width="50%" title="Github_Logo"/> <br>


# MongoDB Atlas Training for LGU Plus Dax


### Compass

MongoDB Cluster에 접속하여 저장된 데이터 등을 볼 수 있는 개발자용 GUI툴입니다. 이를 이용하여 데이터를 조회 하고 변경 하여 줍니다. 다음 링크에서 다운로드가 가능 합니다.    
Compass :   
https://www.mongodb.com/products/compass

Mongosh을 이용하여 Atlas와 연결하여 데이터를 생성합니다.

먼저 데이터베이스를 선택하여야 합니다. (이미 해당 데이터베이스를 사용 하고 있으면 생략 합니다)
````
Atlas atlas-gamf6g-shard-0 [primary] > use handson
switched to db handson
Atlas atlas-gamf6g-shard-0 [primary] handson>
````

데이터를 생성 하여 주기 위해 다음을 실행 하여 줍니다.   
````
Atlas atlas-gamf6g-shard-0 [primary] handson> for (i=0;i<100;i++) {
 const newUser = {
             ssn:"123-456-000"+i, 
             email:"user"+i+"@email.com", 
             name:"Gildong Hong "+i, 
             age: Math.floor(Math.random()*100),
             DateOfBirth: "1st Jan.", 
             Hobbies:["Martial arts"],
             Addresses:[{"Address Name":"Work","Street":"431, Teheran-ro GangNam-gu ","City":"Seoul", "Zip":"06159"}], 
             Phones:[{"type":"mobile","number":"010-5555-1234"}]
           };
           db.user.insertOne(newUser);
 }
{
  acknowledged: true,
  insertedId: ObjectId("659e5627a72e1f57957a78c9")
}
````

데이터가 100건이 생성이 되게 됩니다.


#### Connection
MongoDB atlas Console에 접근 주소를 얻어야 합니다. 
접속 주소를 얻기 위해 Console에 로그인합니다.    
데이터베이스 클러스터의 Connect 버튼을 클릭 합니다.

<img src="/01.aggregation/images/image01.png" width="90%" height="90%">     

접근방법을 선택 하여 주는 단계에서 Connect using MongoDB Compass를 선택 하면 접근 주소를 얻을 수 있습니다.    

<img src="/01.aggregation/images/image02.png" width="60%" height="60%">     

Connection String을 복사하여 줍니다. 이후 Compass를 실행 하여 줍니다.     
<img src="/01.aggregation/images/image03.png" width="70%" height="70%">     



복사한 Connection String을 입력하여 줍니다.   

<img src="/01.aggregation/images/image04.png" width="90%" height="90%">     




#### MQL


### Aggregation

Movies 컬렉션에서 장르가 "Comedy" 인 영화 중 포함된 모든 국가를 기준으로 그룹하여 국가별 포함 개수를 "CountriesInComedy" 컬렉션에 데이터를 생성하여 줍니다.  

Aggregation 이 제공하는 Stage 중, Match 를 이용하여 장르가 Comedy인 것을 찾을 수 있으며, 배열로 되어 있는 항목을 개별로 전환은 unwind 를 이용합니다. 국가별로 그룹을 만들기 위해서는 group Stage를 활용 하며 결과 데이터를 컬렉션에 넣기 위해서는 out을 이용 합니다.   

matach
Find와 유사한 형태로 사용 합니다.

````
{$match: 
  {
    genres: 'Comedy',
  }
}
````

unwind
배열을 항목을 지정하면 이를 개별 문서로 전환 하여 줍니다.  

````
{$unwind: 
  {
    path: '$countries',
  }
}
````

group
지정된 필드를 기준으로 그룹하여 줍니다. SQL의 Group by 와 유사 합니다. 그룹에 따른 계산은 그룹별 카운트 한 횟수로 합니다. 

````
{$group: 
  {
    _id: '$countries',
    count: {
      $sum: 1,
    }
  }
}
````

out
입력된 커서를 지정된 컬렉션으로 생성 하여 줍니다.

````
{
    $out: 'countriesByComedy',
}
````

Compass 의 Aggregation에서 Stage를 생성 하여 줍니다.   

match stage 생성 하기   

<img src="/01.aggregation/images/image25.png" width="90%" height="90%">     

unwind stage 생성 하기    

<img src="/01.aggregation/images/image26.png" width="90%" height="90%">    

group stage 생성 하기    

<img src="/01.aggregation/images/image27.png" width="90%" height="90%">     

out stage 생성 하기    

<img src="/01.aggregation/images/image28.png" width="90%" height="90%">     

생성된 컬렉션을 확인 합니다. out은 컬렉션을 생성하고 데이터를 생성 하여 줌으로 다시 aggregation을 실행 하기 위해서는 생성된 컬렉션을 삭제하고 실행 해줍니다. (실행 후 작성한 aggregation을 저장하여 줍니다.)

<img src="/01.aggregation/images/image29.png" width="100%" height="100%">     


