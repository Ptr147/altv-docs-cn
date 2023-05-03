# 主列表API   

主列表API允许您从alt:V主列表服务获取数据。

## 信息  

* 仅GET  
* 返回JSON对象  
* 如果滥用,您可能会被禁止使用API  

## API链接  

|                   URL                      |                                    描述                                                      |
| :----------------------------------------: | :-----------------------------------------------------------------------------------------: |
| https://api.alt-mp.com/servers               | 统计信息 - 所有服务器上的玩家数量 & 在线服务器数量                             |
| https://api.alt-mp.com/servers/list          | 服务器 - 有关所有服务器的所有已知信息(名称、描述、IP、语言、网站等)      |  
| https://api.alt-mp.com/server/MASTERLIST_ID  | 特定服务器 - 过滤服务器列表以选择一个特定服务器。使用服务器令牌唯一的"id" |
| https://api.alt-mp.com/avg/MASTERLIST_ID/TIME| 平均值 - 返回有关指定服务器的平均数据(TIME = 1d, 7d, 31d)                    |
| https://api.alt-mp.com/max/MASTERLIST_ID/TIME| 最大值 - 返回有关指定服务器的最大数据(TIME = 1d, 7d, 31d)                      |

## 响应示例  
### 统计信息

```json
{ "ServersCount": 64, "playersCount": 1582 }
```

### 服务器列表
这是一个API响应的片段。

```json
[
    {
        "id":"ceaac3d1cc22761223beac38386f5ab2",
        "maxPlayers":128,
        "players":0,
        "name":"Simple Freeroam Server | by PlebMasters.de",
        "locked":false,
        "host":"116.203.227.203",
        "port":7791,
        "gameMode":"Freeroam",
        "website":"discord.gg/86uqkqc",
        "language":"en",
        "description":"A simple game mode to test the performance of alt:V, but also to move freely around the map without any rules.",
        "verified":false,
        "promoted":false,
        "useEarlyAuth":false,
        "earlyAuthUrl":"",
        "useCdn":false,
        "cdnUrl":"",
        "useVoiceChat":false,
        "tags":
            [
                "No Rules",
                "All Weapons",
                "All Vehicles"
            ],
        "bannerUrl":null,
        "branch":"release",
        "build":1181,
        "lastUpdate":1596840894273
    }
]
```

### 特定服务器
```json
{
    "active":true,
    "info":{
        "id":"ceaac3d1cc22761223beac38386f5ab2",
        "maxPlayers":128,
        "players":0,
        "name":"Simple Freeroam Server | by PlebMasters.de",
        "locked":false,
        "host":"144.91.81.134",
        "port":7791,
        "gameMode":"Freeroam",
        "website":"discord.gg/86uqkqc",
        "language":"en",
        "description":"A simple game mode to test the performance of alt:V, but also to move freely around the map without any rules.",
        "verified":false,
        "promoted":false,
        "useEarlyAuth":false,
        "earlyAuthUrl":"",
        "useCdn":false,
        "cdnUrl":"",
        "useVoiceChat":false,
        "tags":[
            "No Rules",
            "All Weapons",
            "All Vehicles",
            "Enabled Cayo Perico"
        ],
        "bannerUrl":null,
        "branch":"release",
        "build":-1,
        "version":"2.8",
        "lastUpdate":1612571971068
    }
}
```

### 平均值/最大值
```json
[
    { "t":1612562230,"c":4 },
    { "t":1612562280,"c":4 },
    { "t":1612562341,"c":4 }
]
```

描述:t = UTC时间戳,c = 玩家数量。

# altstats.net - alt:V Stats API(非官方)

altstats.net - alt:V Stats 是一个替代的服务器列表,由 altMP 团队的成员开发和支持。它具有有关服务器的附加统计信息,例如过去24小时、7天或一个月的玩家数量,并且您可以使用服务器特定的API。 

显示过去一个月的玩家数量等功能属于付费功能,您需要付费才能启用它们。  
## 信息

* 仅GET
* 返回JSON对象
* 如果滥用,您可能会被禁止使用API 
* API可以随时更改而不另行通知

## API 链接

|                     URL                         |                           描述                           |
| :---------------------------------------------: | :-------------------------------------------------------------: |
| https://api.altstats.net/api/v1/master          | 数量 - 显示过去24小时服务器和玩家数量 |
| https://api.altstats.net/api/v1/server          | 服务器 - 有关所有已知服务器的基本信息             |
| https://api.altstats.net/api/v1/server/serverID | 特定服务器 - 列出有关服务器的所有信息      |

##  响应示例
### 数量
```json
[
  {
    "ServerCount": 72,
    "PlayerCount": 958,
    "TimeStamp": "2021-01-01T12:15:00.464Z"
  },
  {
    "ServerCount": 73,
    "PlayerCount": 945,
    "TimeStamp": "2021-01-01T12:10:00.465Z"
  },
  {
    "others": "..."
  }
]
```

### 服务器列表
```json
[
  {
    "others": "..."
  },
  {
    "id": 330,
    "name": "Simple Freeroam Server | by PlebMasters.de",
    "locked": 0,
    "playerCount": 1,
    "slots": 128,
    "gameMode": "Freeroam",
    "language": {
      "full": "English",
      "short": "gb"
    },
    "official": 0,
    "verified": 0,
    "promoted": 0,
    "tags": [
      "No Rules",
      "All Weapons",
      "All Vehicles",
      "Enabled Cayo Perico"
    ]
  },
  {
    "others": "..."
  }
]
```

### 特定 服务器
```json
{
  "Id": 330,
  "FoundAt": "2020-01-01T12:41:21.433Z",
  "LastActivity": "2021-01-01T12:00:19.364Z",
  "Visible": true,
  "ServerId": "ceaac3d1cc22761223beac38386f5ab2",
  "Players": 1,
  "Name": "Simple Freeroam Server | by PlebMasters.de",
  "Locked": false,
  "Ip": "144.91.81.134",
  "Port": 7791,
  "MaxPlayers": 128,
  "Ping": 0,
  "Website": "discord.gg/86uqkqc",
  "Language": "English",
  "Description": "A simple game mode to test the performance of alt:V, but also to move freely around the map without any rules.",
  "LastUpdate": 1609502415186,
  "IsOfficial": false,
  "PlayerRecord": 34,
  "PlayerRecordDate": "2020-07-01T14:15:07.388Z",
  "LastFetchOnline": true,
  "LanguageShort": "gb",
  "GameMode": "Freeroam",
  "Branch": "release",
  "Build": -1,
  "CdnUrl": "",
  "EarlyAuthUrl": "",
  "Verified": false,
  "UseCdn": false,
  "UseEarlyAuth": false,
  "BannerUrl": null,
  "Promoted": false,
  "Tags": [
    "No Rules",
    "All Weapons",
    "All Vehicles",
    "Enabled Cayo Perico"
  ],
  "UseVoiceChat": false,
  "Level": 3,
  "Version": "2.2"
}
```