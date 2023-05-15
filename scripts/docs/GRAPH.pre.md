# Getting started

Welcome to the **{{ graph.name }}** API! 🎉 Get familiar with available objects in the [Schema Reference]({{ graph.url.reference }}), or try querying this graph using [Explorer]({{ graph.url.explorer }}).

Note: beside [Explorer]({{ graph.url.explorer }}) which not support upload file, you can use Altair app for test api, this app support test upload file to server: download app for chrome [here]({{ altair.url.download }}).

# What this graph is all about

Describle how to implement api of rent-room backend app🦄🌌✨.

You can find the schema of database [here]({{ database.url.schema }}).

# Accessing the graph

🛰 You can send operations to this graph at `{{ graph.url.endpoint }}` by using whatever app like [postman]({{ postman.url.download }}), [altair]({{ altair.url.download }}) or default [Explorer]({{ graph.url.explorer }}) .
📇 The Apollo Registry holds the canonical location of your schema. In the registry, this graph is referred to by its “graph ref”, which is: **{{ graph.ref }}**.

*(Note: you can [download Rover](https://www.apollographql.com/docs/rover/getting-started/), the Apollo CLI tool for working with your schema locally.)*

# Authentication

## How to register to this graph

Go to [Postman]({{ postman.url.download }}) app or app that support cookie (not the [Explorer]({{ graph.url.explorer }}) because it doesn't not support cookie) and type. Server will generate a cookie that save access token and refresh token in http-only cookie.

```gql
mutation Register($input: UserCreateInput!) {
  register(input: $input) {
    ... on User {
      _id
      email
      fullname
      numberPhone
      province
      district
      ward
      provinceName
      districtName
      wardName
      avatar
      userType
      role
      createdAt
      updatedAt
    }
    ... on EmailDuplicateError {
      errorCode
      message
    }
    ... on PasswordInvalidError {
      errorCode
      message
    }
  }
}
```

```
{
  "input": {
    "email": "test2@gmail.com",
    "password": "1234",
    "fullname": "Cao Trung Hiếu",
    "numberPhone": "0977157490",
    "province": 1,
    "district": 2,
    "ward": 3,
    "avatar": null,
    "userType": "TENANT"
  }
}
```

## How to authenticate to this graph

Go to [Postman]({{ postman.url.download }}) app or app that support cookie (not the [Explorer]({{ graph.url.explorer }}) because it doesn't not support cookie) and type. Server will generate a cookie that save access token and refresh token in http-only cookie.

```gql
query Login($email: String!, $password: String!) {
  login(email: $email, password: $password) {
    ... on User {
      _id
      email
      fullname
      numberPhone
      province
      district
      ward
      provinceName
      districtName
      wardName
      avatar
      userType
      role
      createdAt
      updatedAt
    }
    ... on EmailNotRegisterError {
      errorCode
      message
    }
    ... on PasswordIncorrectError {
      errorCode
      message
    }
  }
}
```

```
{  
  "email": "test21@gmail.com",
  "password": "1234"
}
```

## How to logout

Go to [Postman]({{ postman.url.download }}) app or app that support cookie (not the [Explorer]({{ graph.url.explorer }}) because it doesn't not support cookie) and type. Server will delete cookie in http-only cookie.

```gql
mutation Logout {
  logout {
    ... on UserNotAuthenticatedError {
      errorCode
      message
    }
    ... on LogoutStatus {
      success
    }
  }
}
```

## Get profile

This operator will check access token and refresh token in cookies of request to get current user.

```gql
query Profile {
  profile {
    user {
      _id
      email
      fullname
      numberPhone
      province
      district
      ward
      provinceName
      districtName
      wardName
      avatar
      userType
      role
      createdAt
      updatedAt
    }
    isAuth
  }
}
```


# Running operations

Some basic operations you can try:

*(Note: try this query in [Postman]({{ postman.url.download }}) because it support cookie)*

## Get the list

> Get all homes information:

```gql
query AllHomes($paginatorOptions: PaginatorOptionsInput) {
  allHomes(paginatorOptions: $paginatorOptions) {
    docs {
      _id
      owner {
        email
        _id
      }
      province
      district
      ward
      provinceName
      districtName
      wardName
      liveWithOwner
      electricityPrice
      waterPrice
      internetPrice
      cleaningPrice
      images
      totalRooms
      listRooms {
        docs {
          _id
        }
      }
      position {
        x
        y
        lng
        lat
      }
      description
      detailAddress
      title
      minPrice
      maxPrice
      createdAt
      updatedAt
    }
    paginator {
      totalDocs
      limit
      page
      nextPage
      prevPage
      totalPages
      pagingCounter
      hasPrevPage
      hasNextPage
    }
  }
}
```

```
{
  "paginatorOptions": {
    "page": 1,
    "limit": 10,
    "sort": [
      {
        "field": "createdAt",
        "arrange": "ASC"
      }
    ]
  }
}
```

> Get all rooms information:

```gql
query AllRooms($paginatorOptions: PaginatorOptionsInput) {
  allRooms(paginatorOptions: $paginatorOptions) {
    docs {
      _id
      home {
        _id
      }
      price
      square
      isRented
      floor
      images
      description
      roomNumber
      title
      amenities
      createdAt
      updatedAt
    }
    paginator {
      totalDocs
      limit
      page
      nextPage
      prevPage
      totalPages
      pagingCounter
      hasPrevPage
      hasNextPage
    }
  }
}
```

```
{
  "paginatorOptions": {
    "page": 1,
    "limit": 10,
    "sort": [
      {
        "field": "createdAt",
        "arrange": "ASC"
      }
    ]
  }
}
```

Basically, homes_list and rooms_list implement interface PaginatorResult that have two fields: docs and paginator, that for pagination.

```gql
interface PaginatorResult {
    docs: [Result]
    paginator: Paginator
}
```

## Get the item:

> Get a home by home_id:

```gql
query GetHomeById($getHomeByIdId: ID!) {
  getHomeById(id: $getHomeByIdId) {
    ... on Home {
      _id
      owner {
        _id
      }
      province
      district
      ward
      provinceName
      districtName
      wardName
      liveWithOwner
      electricityPrice
      waterPrice
      internetPrice
      cleaningPrice
      images
      totalRooms
      listRooms {
        docs {
          _id
        }
      }
      description
      detailAddress
      title
      minPrice
      maxPrice
      createdAt
      updatedAt
      position {
        x
        y
        lng
        lat
      }
    }
    ... on InstanceNotExistError {
      errorCode
      message
    }
  }
}
```

```
{
  "getHomeByIdId": "id"
}
```

> Get a room by room_id:

```gql
query GetRoomById($getRoomByIdId: ID!) {
  getRoomById(id: $getRoomByIdId) {
    ... on Room {
      _id
      home {
        _id
      }
      price
      square
      isRented
      floor
      images
      description
      roomNumber
      title
      amenities
      createdAt
      updatedAt
    }
    ... on InstanceNotExistError {
      errorCode
      message
    }
  }
}
```

```
{
  "getRoomByIdId": "id"
}
```

## Basic CRUD for home and room:

*(Note: some varibale that has '$' prefix is a variable of input. In [Explorer]({{ graph.url.explorer }}) you must pass this varibale to variable-part)*

### Home

> Create a home:

```gql
mutation CreateHome($input: HomeCreateInput!) {
  createHome(input: $input) {
    ... on Home {
      _id
      owner {
        _id
      }
      province
      district
      ward
      provinceName
      districtName
      wardName
      liveWithOwner
      electricityPrice
      waterPrice
      internetPrice
      cleaningPrice
      images
      totalRooms
      listRooms {
        docs {
          _id
        }
      }
      position {
        x
        y
        lng
        lat
      }
      description
      detailAddress
      title
      minPrice
      maxPrice
      createdAt
      updatedAt
    }
    ... on PermissionDeninedError {
      errorCode
      message
    }
  }
}
```

```
{
  "input": {
    "province": null,
    "district": null,
    "ward": null,
    "liveWithOwner": null,
    "electricityPrice": null,
    "waterPrice": null,
    "internetPrice": null,
    "cleaningPrice": null,
    "images": null,
    "totalRooms": null,
    "position": {
      "x": null,
      "y": null,
      "lng": null,
      "lat": null
    },
    "detailAddress": null,
    "description": null,
    "title": null,
    "minPrice": null,
    "maxPrice": null
  }
}
```

> Update a home:

```gql
mutation UpdateHome($input: HomeUpdateInput!) {
  updateHome(input: $input) {
    ... on Home {
      _id
      owner {
        _id
      }
      province
      district
      ward
      provinceName
      districtName
      wardName
      liveWithOwner
      electricityPrice
      waterPrice
      internetPrice
      cleaningPrice
      images
      totalRooms
      position {
        x
        y
        lng
        lat
      }
      description
      detailAddress
      title
      minPrice
      maxPrice
      createdAt
      updatedAt
    }
    ... on InstanceNotExistError {
      errorCode
      message
    }
    ... on PermissionDeninedError {
      errorCode
      message
    }
  }
}
```

```
{
  "input": {
    "id": null,
    "province": null,
    "district": null,
    "ward": null,
    "liveWithOwner": null,
    "electricityPrice": null,
    "waterPrice": null,
    "internetPrice": null,
    "cleaningPrice": null,
    "images": null,
    "totalRooms": null,
    "position": {
      "x": null,
      "y": null,
      "lng": null,
      "lat": null
    },
    "detailAddress": null,
    "description": null,
    "title": null,
    "minPrice": null,
    "maxPrice": null
  }
}
```

> Delete a home:

```gql
mutation DeleteHome($deleteHomeId: ID!) {
  deleteHome(id: $deleteHomeId) {
    ... on AfterDelete {
      id
      success
    }
    ... on InstanceNotExistError {
      errorCode
      message
    }
    ... on PermissionDeninedError {
      errorCode
      message
    }
  }
}
```

```
{
  "deleteHomeId": "id"
}
```

### Room

> Create a room:

```gql
mutation CreateRoom($input: RoomCreateInput!) {
  createRoom(input: $input) {
    ... on Room {
      _id
      home {
        _id
      }
      price
      square
      isRented
      floor
      images
      description
      roomNumber
      title
      amenities
      createdAt
      updatedAt
    }
    ... on PermissionDeninedError {
      errorCode
      message
    }
  }
}
```

```
{
  "input": {
    "home": null,
    "price": null,
    "square": null,
    "isRented": null,
    "floor": null,
    "images": null,
    "description": null,
    "roomNumber": null,
    "title": null,
    "amenities": null
  }
}
```

> Update a room:

```gql
mutation UpdateRoom($input: RoomUpdateInput!) {
  updateRoom(input: $input) {
    ... on Room {
      _id
      home {
        _id
      }
      price
      square
      isRented
      floor
      images
      description
      roomNumber
      title
      amenities
      createdAt
      updatedAt
    }
    ... on InstanceNotExistError {
      errorCode
      message
    }
    ... on PermissionDeninedError {
      errorCode
      message
    }
  }
}
```

```
{
  "input": {
    "id": null,
    "price": null,
    "square": null,
    "isRented": null,
    "floor": null,
    "images": null,
    "description": null,
    "roomNumber": null,
    "title": null,
    "amenities": null
  }
}
```

> Delete a room

```gql
mutation DeleteRoom($deleteRoomId: ID!) {
  deleteRoom(id: $deleteRoomId) {
    ... on AfterDelete {
      id
      success
    }
    ... on InstanceNotExistError {
      errorCode
      message
    }
    ... on PermissionDeninedError {
      errorCode
      message
    }
  }
}
```

```
{
  "deleteRoomId": "id"
}
```

# Report
- Link: [report]({{ report.url }})

# Getting help with this graph

For support working with this graph, contact the Graph Admin via [gmail]({{ admin.gmail }}) or [messenger]({{ admin.url.messenger }}).


