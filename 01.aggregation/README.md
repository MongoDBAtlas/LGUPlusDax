<img src="https://companieslogo.com/img/orig/MDB_BIG-ad812c6c.png?t=1648915248" width="50%" title="Github_Logo"/> <br>


# MongoDB Atlas Training for LGU Plus AICC

### [&rarr; CRUD with Mongosh](#MONGOSH)

### [&rarr; Compass 를 이용한 데이터 확인](#Compass)

### [&rarr; 추가 Query](#option)

### [&rarr; Aggregation](#Aggregation)

### [&rarr; Lookup 을 이용한 조인](#Lookup)

### [&rarr; 추가 Aggregation](#option)

<br>


### MONGOSH

Mongosh로 Atlas 에 접속 하고 MongoDB Query 를 이용하여 데이터를 생성, 조회, 삭제를 테스트 합니다.


#### Connection

MongoDB Atlas 와 Mongosh을 이용하여 연결 합니다.    
MongoDB atlas Mongosh 접근 주소를 얻어야 합니다. 
접속 주소를 얻기 위해 Console에 로그인합니다. 
데이터베이스 클러스터의 Connect 버튼을 클릭 합니다.

<img src="/01.aggregation/images/image01.png" width="90%" height="90%">     


접근방법을 선택 하여 주는 단계에서 Shell을 선택 하면 접근 주소를 얻을 수 있습니다.   

<img src="/01.aggregation/images/image20.png" width="60%" height="60%">   

Mongosh이 설치 되어 있음으로 I have the MongoDB Shell installed를 선택하고 계정 접근은 암호로 접근할 것임으로 Password를 선택하면 접근 할 수 있는 주소를 얻을 수 있습니다.    

<img src="/01.aggregation/images/image21.png" width="70%" height="70%">     


Terminal을 열고 해당 주소를 이용하여 mongosh를 실행 하여 줍니다. (접근하기 위한 Account로 입력 하여 줍니다.)

````
 % mongosh "mongodb+srv://cluster0.5qjlg.mongodb.net/myFirstDatabase" --apiVersion 1 --username atlas-account    
Enter password: **********
Current Mongosh Log ID:	64454459813babb209a83f4c
Connecting to:		mongodb+srv://cluster0.5qjlg.mongodb.net/myFirstDatabase
Using MongoDB:		6.0.5 (API Version 1)
Using Mongosh:		1.0.5

For mongosh info see: https://docs.mongodb.com/mongodb-shell/

Atlas atlas-t0pzlo-shard-0 [primary] myFirstDatabase> 
````

#### Insert Test

Mongosh을 이용하여 Atlas와 연결하여 데이터를 생성 합니다.

먼저 데이터베이스를 선택하여야 합니다.
````
Atlas atlas-gamf6g-shard-0 [primary] MMT> use handson
switched to db handson
Atlas atlas-gamf6g-shard-0 [primary] handson>
````

다음 데이터 베이스 명령으로 데이터를 생성 합니다.

````
db.user.insertOne(
  {
        ssn:"123-456-0001", 
        email:"user@email.com", 
        name:"Gildong Hong", 
        DateOfBirth: "1st Jan.", 
        Hobbies:["Martial arts"],
        Addresses:[{"Address Name":"Work","Street":"431, Teheran-ro GangNam-gu ","City":"Seoul", "Zip":"06159"}], 
        Phones:[{"type":"mobile","number":"010-5555-1234"}]
  }
)

{
  acknowledged: true,
  insertedIds: { '0': ObjectId("653662c14771f95974766b8d") }
}
````
Atlas Console 에서 데이터 생성 여부를 확인 합니다.


#### find Test

Mongosh을 이용하여 Atlas와 연결하여 데이터를 조회 합니다.

먼저 데이터베이스를 선택하여야 합니다. (이미 해당 데이터베이스를 사용 하고 있으면 생략 합니다)
````
Atlas atlas-gamf6g-shard-0 [primary] MMT> use handson
switched to db handson
Atlas atlas-gamf6g-shard-0 [primary] handson>
````

다음 데이터를 추가 한 후 테스트 진행 합니다.

````
let users = 
[
  {
    ssn: '123-456-0002',
    email: 'Sejong@email.com',
    name: 'Sejong King',
    DateOfBirth: '31th Jan.',
    Hobbies: [ 'Reading', 'Invent' ],
    Addresses: [
      {
        'Address Name': 'Work',
        Street: '431, Kyungbok Palice ',
        City: 'Seoul'
      }
    ]
  },
  {
    ssn: '123-456-0003',
    email: 'Soonshin@email.com',
    name: 'Soonshin General',
    DateOfBirth: '3th Jan.',
    Hobbies: [ 'Martial arts' ],
    Addresses: [ { 'Address Name': 'Work', Street: 'Noryang', City: 'ChongMu' } ]
  },
  {
    ssn: '123-456-0004',
    email: 'Micky@email.com',
    name: 'Michey',
    DateOfBirth: '5th Jan.',
    Hobbies: [ 'Playing' ],
    Addresses: [
      {
        'Address Name': 'Work',
        Street: 'Disney land',
        City: 'Anaheim',
        State: 'CA'
      }
    ]
  }
]


db.user.insertMany(users)
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("65595fa36eedc4aee55bdba9"),
    '1': ObjectId("65595fa36eedc4aee55bdbaa"),
    '2': ObjectId("65595fa36eedc4aee55bdbab")
  }
}

````

SSN을 이용한 기본 검색으로 "123-456-0001" 인 데이터를 조회 합니다
````
db.user.find({ssn:"123-456-0001"});

[
  {
    _id: ObjectId("64454591813babb209a83f4d"),
    ssn: '123-456-0001',
    email: 'user@email.com',
    name: 'Gildong Hong',
    DateOfBirth: '1st Jan.',
    Hobbies: [ 'Martial arts' ],
    Addresses: [
      {
        'Address Name': 'Work',
        Street: '431, Teheran-ro GangNam-gu ',
        City: 'Seoul',
        Zip: '06159'
      }
    ],
    Phones: [ { type: 'mobile', number: '010-5555-1234' } ]
  }
]
````

직장이 서울인 사람 중 취미 정보가 Martial arts 인 사람을 중 email 과 이름, 주소 점보만 출력합니다. (Nested Document에 대한 검색)
Addresses 하위로 Sub-document 가 있으며 City 항목에 도시 정보를 포함하고 있어 이를 이용하여 검색을 진행 합니다.   

````
let query = {$and: [{"Addresses.City":"Seoul"}, {Hobbies:"Martial arts"}]}
let projection = {email:1, name:1,Addresses:1, _id:0}

db.user.find(query, projection);
[
  {
    email: 'user@email.com',
    name: 'Gildong Hong',
    Addresses: [
      {
        'Address Name': 'Work',
        Street: '431, Teheran-ro GangNam-gu ',
        City: 'Seoul',
        Zip: '06159'
      }
    ]
  }
]

````



#### Update Test

Mongosh을 이용하여 Atlas와 연결하여 데이터를 업데이트 합니다.

먼저 데이터베이스를 선택하여야 합니다. (이미 해당 데이터베이스를 사용 하고 있으면 생략 합니다)
````
Atlas atlas-gamf6g-shard-0 [primary] > use handson
switched to db handson
Atlas atlas-gamf6g-shard-0 [primary] handson>
````

수정할 데이터를 ssn을 입력 하여 줍니다.
수정 대상 데이터의 ssn 및 수정할 데이터 항목을 확인 수정 하여 줍니다.
`````
db.user.updateOne(
  {"ssn":"123-456-0001"},
  { $set: { email: "gildong@email.com" } }
);

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}      
`````

데이터를 수정 결과를 확인 합니다. (이메일 주소가 수정 된 것을 확인 합니다)
````
db.user.find({"ssn":"123-456-0001"});

[
  {
    _id: ObjectId("64454591813babb209a83f4d"),
    ssn: '123-456-0001',
    email: 'gildong@email.com',
    name: 'Gildong Hong',
    DateOfBirth: '1st Jan.',
    Hobbies: [ 'Martial arts' ],
    Addresses: [
      {
        'Address Name': 'Work',
        Street: '431, Teheran-ro GangNam-gu ',
        City: 'Seoul',
        Zip: '06159'
      }
    ],
    Phones: [ { type: 'mobile', number: '010-5555-1234' } ]
  }
]
````

#### Update Hobbies Test

Mongosh을 이용하여 Atlas와 연결하여 데이터를 업데이트 (Hobbies를 추가)합니다.

먼저 데이터베이스를 선택하여야 합니다. (이미 해당 데이터베이스를 사용 하고 있으면 생략 합니다)
````
Atlas atlas-gamf6g-shard-0 [primary] > use handson
switched to db handson
Atlas atlas-gamf6g-shard-0 [primary] handson>
````

수정할 데이터를 ssn을 입력 하여 줍니다.
수정 대상 데이터의 ssn 및 Hobby 항목을 추가 하여 줍니다. (취미로 Reading 추가 하기)
`````
db.user.updateOne(
  {"ssn":"123-456-0001"},
  { $push: { Hobbies:"Reading" } }
);

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
`````

데이터를 수정 결과를 확인 합니다. (Hobby에 Reading이 추가되어 있음)
````
db.user.find({"ssn":"123-456-0001"});

[
  {
    _id: ObjectId("64454591813babb209a83f4d"),
    ssn: '123-456-0001',
    email: 'gildong@email.com',
    name: 'Gildong Hong',
    DateOfBirth: '1st Jan.',
    Hobbies: [ 'Martial arts', 'Reading' ],
    Addresses: [
      {
        'Address Name': 'Work',
        Street: '431, Teheran-ro GangNam-gu ',
        City: 'Seoul',
        Zip: '06159'
      }
    ],
    Phones: [ { type: 'mobile', number: '010-5555-1234' } ]
  }
]

````

#### Remove Test

Mongosh을 이용하여 Atlas와 연결하여 데이터를 삭제 합니다.

먼저 데이터베이스를 선택하여야 합니다. (이미 해당 데이터베이스를 사용 하고 있으면 생략 합니다)
````
Atlas atlas-gamf6g-shard-0 [primary] > use handson
switched to db handson
Atlas atlas-gamf6g-shard-0 [primary] handson>
````

삭제할 데이터를 수정 하여 줍니다.
삭제할 데이터의 ssn 및 입력 하여줍니다.
`````
db.user.deleteOne({ssn:"123-456-0001"});

{ acknowledged: true, deletedCount: 1 }

`````

데이터를 확인 합니다.
````
db.user.findOne({ssn:"123-456-0001"});
null
````



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


#### 데이터 조회
데이터베이스에서 생성한 handson 탭을 클릭 하면 컬렉션 리스트를 볼 수 있습니다. 생성한 user컬렉션을 선택 합니다.    

<img src="/01.aggregation/images/image05.png" width="90%" height="90%">     

데이터 검색을 위해서 Filter 부분에 검색 조건을 입력 하여 줍니다.
ssn 이 123-456-0001 인 데이터를 찾기 위해 다음과 같이 입력 하여 줍니다.

````
{ssn: "123-456-0001"}
````

<img src="/01.aggregation/images/image06.png" width="90%" height="90%">     

나이(age)가 10 이상 40이하인 사람을 찾기를 합니다. 조건은 age >= 10 이고 age <=40으로 합니다.

````
{age: {$gte: 10, $lte: 40}}
````

<img src="/01.aggregation/images/image07.png" width="90%" height="90%">     



### option
생성된 데이터 베이스중 Movie 관련 데이터 컬렉션 (sample_mflix.movies)에서 다음 내용을 Query 합니다.

- 1987 년에 나온 데이터 조회 (Where year = 1987)

- 장르가 Comedy 에 속하는 영화 검색

- 장르가 Comedy 하나 만 있는 데이터 검색

- 장르가 Comedy 혹은 Drama 인 데이터 검색

- imdb 의 평가 점수가 8.0 이상이고 등급이 PG 인 영화 검색

- metacritic의 평점이 존재 하는 영화 검색

- Dr. Strangelove 로 시작하는 영화 검색

해당 쿼리는 다음과 같습니다.
- 1987 년에 나온 데이터 조회 (Where year = 1987)
````
db.movies.find({year:1987})
````
<img src="/01.aggregation/images/image11.png" width="90%" height="90%">     

- 장르가 Comedy 에 속하는 영화 검색
````
db.movies.find({genres: "Comedy"})

````
<img src="/01.aggregation/images/image12.png" width="90%" height="90%">     

- 장르가 Comedy 하나 만 있는 데이터 검색
````
db.movies.find({genres:["Comedy"]})

````
<img src="/01.aggregation/images/image13.png" width="90%" height="90%">     

- 장르가 Comedy 혹은 Drama 인 데이터 검색
````
db.movies.find({genres:{$in:["Comedy", "Drama"]}})

````
<img src="/01.aggregation/images/image14.png" width="90%" height="90%">     

- imdb 의 평가 점수가 8.0 이상이고, rated 필드 값이 존재할 시 해당 필드 값이 PG 인 영화 검색
````
db.movies.find({"imdb.rating" : {$gt: 8.0}, rated:"PG"})

````
<img src="/01.aggregation/images/image15.png" width="90%" height="90%">     

- metacritic의 평점이 존재 하는 영화 검색
````
db.movies.find({metacritic: {$exists: true}})

````
<img src="/01.aggregation/images/image16.png" width="90%" height="90%">     

- Dr. Strangelove 로 시작하는 영화 검색
````
db.movies.find({title: {$regex: '^Dr. Strangelove'}})

````
<img src="/01.aggregation/images/image17.png" width="90%" height="90%">     

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


### Lookup

sample_mflix.comments 와 sample_mflix.users 를 결합하여 데이터를 조회 합니다.    
users의 데이터 중 이름이 "Mercedes Tyler"인 사람을 찾아 그가 게시한 Comments 를 찾습니다.   

해당 데이터를 검색 하면 다음과 같습니다.    
users    
````
{
  "_id": {
    "$oid": "59b99dedcfa9a34dcd78862d"
  },
  "name": "Mercedes Tyler",
  "email": "mercedes_tyler@fakegmail.com",
  "password": "$2b$12$ONDwIwR9NKF1Tp5GjGI12e8OFMxPELoFrk4x4Q3riJGWY6jl/UZAa"
}
````

comments 의 경우 다음과 같습니다.
````
[{
  "_id": {
    "$oid": "5a9427648b0beebeb69579e7"
  },
  "name": "Mercedes Tyler",
  "email": "mercedes_tyler@fakegmail.com",
  "movie_id": {
    "$oid": "573a1390f29313caabcd4323"
  },
  "text": "Eius veritatis vero facilis quaerat fuga temporibus. Praesentium expedita sequi repellat id. Corporis minima enim ex. Provident fugit nisi dignissimos nulla nam ipsum aliquam.",
  "date": {
    "$date": {
      "$numberLong": "1029646567000"
    }
  }
},
...
]
````

Lookup으로 조인을 하여 데이터를 볼 때는 전체 데이터를 조인 하는 것 보다 Match를 이용하여 Join 할 범위를 좁힌 후에 하는 것이 필요 합니다.   

Aggregation을 작성하기 위해 Compass에서 sample_mflix.users를 선택 합니다.  
Aggregation 탭에서 먼저 match 스테이지를 작성 합니다.

Match
````
{$match:
  {
    name:"Mercedes Tyler"
  }
}
````

Lookup 스테이지를 추가하여 줍니다.    
Lookup
````
{$lookup:
  {
    from: "comments",
    localField: "name",
    foreignField: "name",
    as: "Comments"
  }
}
````

<img src="/01.aggregation/images/image32.png" width="80%" height="80%">     

결과로 다음과 같이 Comments를 포함한 결과가 보여 집니다.


````
db.users.aggregate(
[
  {
    $match: {
      name: "Mercedes Tyler",
    },
  },
  {
    $lookup: {
      from: "comments",
      localField: "name",
      foreignField: "name",
      as: "Comments",
    },
  },
]);


{
  _id: ObjectId("59b99dedcfa9a34dcd78862d"),
  name: 'Mercedes Tyler',
  email: 'mercedes_tyler@fakegmail.com',
  password: '$2b$12$ONDwIwR9NKF1Tp5GjGI12e8OFMxPELoFrk4x4Q3riJGWY6jl/UZAa',
  Comments: [
    {
      _id: ObjectId("5a9427648b0beebeb69579e7"),
      name: 'Mercedes Tyler',
      email: 'mercedes_tyler@fakegmail.com',
      movie_id: ObjectId("573a1390f29313caabcd4323"),
      text: 'Eius veritatis vero facilis quaerat fuga temporibus. Praesentium expedita sequi repellat id. Corporis minima enim ex. Provident fugit nisi dignissimos nulla nam ipsum aliquam.',
      date: 2002-08-18T04:56:07.000Z
    },
    {
      _id: ObjectId("5a9427648b0beebeb6958131"),
      name: 'Mercedes Tyler',
      email: 'mercedes_tyler@fakegmail.com',
      movie_id: ObjectId("573a1392f29313caabcdb8ac"),
      text: 'Dolores nulla laborum doloribus tempore harum officiis. Rerum blanditiis aperiam nemo dignissimos a magni natus. Tenetur suscipit cumque sint dignissimos. Accusantium eveniet consequuntur officia ea.',
      date: 2007-09-21T08:52:00.000Z
    },
    {
      _id: ObjectId("5a9427648b0beebeb69582cb"),
      name: 'Mercedes Tyler',
      email: 'mercedes_tyler@fakegmail.com',
      movie_id: ObjectId("573a1393f29313caabcdbe7c"),
      text: 'Voluptatem ad enim corrupti esse consectetur. Explicabo voluptates quo aperiam deleniti reiciendis. Temporibus aliquid delectus recusandae commodi.',
      date: 2008-05-17T22:55:39.000Z
    },
    {
      _id: ObjectId("5a9427648b0beebeb69582cc"),
      name: 'Mercedes Tyler',
      email: 'mercedes_tyler@fakegmail.com',
      movie_id: ObjectId("573a1393f29313caabcdbe7c"),
      text: 'Fuga nihil dolor veniam repudiandae. Rem debitis ex porro dolorem maxime laborum. Esse molestias accusamus provident unde. Sint cupiditate cumque corporis nulla explicabo fuga.',
      date: 2011-03-01T12:06:42.000Z
    },
    {
      _id: ObjectId("5a9427648b0beebeb69588e6"),
      name: 'Mercedes Tyler',
      email: 'mercedes_tyler@fakegmail.com',
      movie_id: ObjectId("573a1393f29313caabcde00c"),
      text: 'Et quas doloribus ipsum sapiente amet enim optio. Magni odio pariatur quos. Voluptatum error ipsum nemo similique error vel.',
      date: 1971-05-13T02:38:19.000Z
    },
    {
      _id: ObjectId("5a9427648b0beebeb69589a1"),
      name: 'Mercedes Tyler',
      email: 'mercedes_tyler@fakegmail.com',
      movie_id: ObjectId("573a1393f29313caabcde4a8"),
      text: 'Ipsam quos magnam ipsum odio aspernatur voluptas nihil nesciunt. Deserunt magni corporis aperiam. Delectus blanditiis eius molestiae modi velit illo veritatis.',
      date: 2015-12-10T21:26:15.000Z
    },
    {
      _id: ObjectId("5a9427648b0beebeb6958aeb"),
      name: 'Mercedes Tyler',
      email: 'mercedes_tyler@fakegmail.com',
      movie_id: ObjectId("573a1394f29313caabcde63e"),
      text: 'Magnam repudiandae ipsam perspiciatis. Tenetur commodi tenetur dolorem tempora. Quas a quos laboriosam.',
      date: 2007-09-19T02:17:40.000Z
    },
    ...
  ]
}
````



#### Aggregation Bucket
화가의 프로파일 정보에서 태어난 년도를 기준으로 하여 그룹을 생성 합니다. 년도는 10년을 기준으로 집계 합니다. 즉 1840 ~1850 년으로 집계 합니다.

````
db.artists.insertMany([
  { "_id" : 1, "last_name" : "Bernard", "first_name" : "Emil", "year_born" : 1868, "year_died" : 1941, "nationality" : "France" },
  { "_id" : 2, "last_name" : "Rippl-Ronai", "first_name" : "Joszef", "year_born" : 1861, "year_died" : 1927, "nationality" : "Hungary" },
  { "_id" : 3, "last_name" : "Ostroumova", "first_name" : "Anna", "year_born" : 1871, "year_died" : 1955, "nationality" : "Russia" },
  { "_id" : 4, "last_name" : "Van Gogh", "first_name" : "Vincent", "year_born" : 1853, "year_died" : 1890, "nationality" : "Holland" },
  { "_id" : 5, "last_name" : "Maurer", "first_name" : "Alfred", "year_born" : 1868, "year_died" : 1932, "nationality" : "USA" },
  { "_id" : 6, "last_name" : "Munch", "first_name" : "Edvard", "year_born" : 1863, "year_died" : 1944, "nationality" : "Norway" },
  { "_id" : 7, "last_name" : "Redon", "first_name" : "Odilon", "year_born" : 1840, "year_died" : 1916, "nationality" : "France" },
  { "_id" : 8, "last_name" : "Diriks", "first_name" : "Edvard", "year_born" : 1855, "year_died" : 1930, "nationality" : "Norway" }
]);
````

태어난 년도를 기준으로 하여 집계를 위해서 bucket을 이용하여 groupBy 항목으로 year_born을 하여 줍니다. 태어난 년도의 집계는 10년을 기준으로 category화는 boundaries에 작성 기준을 작성하여 줍니다. 

````
db.artists.aggregate( [
  {
    $bucket: {
      groupBy: "$year_born",                        // Field to group by
      boundaries: [ 1840, 1850, 1860, 1870, 1880 ], // Boundaries for the buckets
      default: "Other",                             // Bucket ID for documents which do not fall into a bucket
      output: {                                     // Output for each bucket
        "count": { $sum: 1 },
        "artists" :
          {
            $push: {
              "name": { $concat: [ "$first_name", " ", "$last_name"] },
              "year_born": "$year_born"
            }
          }
      }
    }
  }
] );


{
  _id: 1840,
  count: 1,
  artists: [
    {
      name: 'Odilon Redon',
      year_born: 1840
    }
  ]
}
{
  _id: 1850,
  count: 2,
  artists: [
    {
      name: 'Vincent Van Gogh',
      year_born: 1853
    },
    {
      name: 'Edvard Diriks',
      year_born: 1855
    }
  ]
}
{
  _id: 1860,
  count: 4,
  artists: [
    {
      name: 'Emil Bernard',
      year_born: 1868
    },
    {
      name: 'Joszef Rippl-Ronai',
      year_born: 1861
    },
    {
      name: 'Alfred Maurer',
      year_born: 1868
    },
    {
      name: 'Edvard Munch',
      year_born: 1863
    }
  ]
}
{
  _id: 1870,
  count: 1,
  artists: [
    {
      name: 'Anna Ostroumova',
      year_born: 1871
    }
  ]
}

````

#### Aggregation Unwind
다음과 같은 의류 정보가 있을 때 의류를 기준으로 가능한 사이즈 정보가 배열화 되어 있습니다. 각 사이즈를 구분하여 문서화를 합니다.    
사이즈가 없는 의류들은 이를 포함 함니다.

````
db.clothing.insertMany([
  { "_id" : 1, "item" : "Shirt", "sizes": [ "S", "M", "L"] },
  { "_id" : 2, "item" : "Shorts", "sizes" : [ ] },
  { "_id" : 3, "item" : "Hat", "sizes": "M" },
  { "_id" : 4, "item" : "Gloves" },
  { "_id" : 5, "item" : "Scarf", "sizes" : null }
]);
````
배열로 되어 있는 값을 하나의 문서로 만들어 주기 위해 unwind를 사용합니다. 기본적으로 지정된 array (size)에 값이 없는 경우 연산에서 제외 합니다. 이를 포함하도록 하는 옵션은 preserveAndEmptyArrays입니다.


````

db.clothing.aggregate( [
   { $unwind: { path: "$sizes", preserveNullAndEmptyArrays: true } }
] );


{
  _id: 1,
  item: 'Shirt',
  sizes: 'S'
}
{
  _id: 1,
  item: 'Shirt',
  sizes: 'M'
}
{
  _id: 1,
  item: 'Shirt',
  sizes: 'L'
}
{
  _id: 2,
  item: 'Shorts'
}
{
  _id: 3,
  item: 'Hat',
  sizes: 'M'
}
{
  _id: 4,
  item: 'Gloves'
}
{
  _id: 5,
  item: 'Scarf',
  sizes: null
}

db.clothing.aggregate( [
   { $unwind: { path: "$sizes" } }
] );
````


