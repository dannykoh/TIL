# JSON Format Query

# Property value query

```json
{
  "_id": "danny"
}
```

위와 같이 `_id`라는 property의 value를 기준으로 찾을 수 있고, 아래와 같이 두 개 이상의 property 값으로 좀 더 narrow down된 리스트를 얻을 수도 있음.

```json
{
  "userType": 1,
  "_id": "danny"
}
```

# Property value that does not equal to keyword

$ne 키워드 사용. 

아래 예시는 type property의 value가 “class”가 아닌 아이템들 query

```json
{
  "type": { "$ne": "class" }
}
```

# Substring of parameter string

특정 property의 파라미터로 substring을 주면 해당 property 중에서 찾으려는 substring을 포함한 아이템들을 찾아줌

```json
{
  "name": { "$regex": "danny", "$options": "i" }
}
```

# Based on numbers greater or lower

숫자 타입의 특정 property value의 크기가 주어진 값보다 크거나 작은 아이템들을 찾기

## Greater

```json
{
  "stone": { "$gt": 1000 }
}
```

## Lower

```json
{
  "stone": { "$lt": 1000 }
}
```

# Items that has or doesn’t have certain property

```json
{
  "propertyName": { "$exists": true }
}
```

# Items that has a property set to either A or B

```json
{
  "status": { "$in": ["active", "pending"] }
}
```