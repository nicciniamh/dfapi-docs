# Database 

The DucksFeet API (dfapi) uses several [MongoDB](https://www.mongodb.com) collections to manage the operations of the dfapi. The choice of [MongoDB](https://www.mongodb.com) is due to it's time-to-live or TTL indexes. Additionally, as comparted with SQL databases, the [MongoDB](https://www.mongodb.com) collection documenta are native dicts in Python and are well well suited for converting to JSON when needed.

It's impoetant to note that when [MongoDB](https://www.mongodb.com) creates a document that does not have an _id an OBJECTID object is used and this type is not serializable to JSON. In the dfapi code the _id is represented as a uuid. 

A [Database Template](#database-template) below shows the structure and default valuues below. 

|Collection| Purpose |
|----------|---------|
|bff_hosts| The hostlist for which the BFF collector caches data |
|host_info| Information about specific host classes |
|lrj_queue *| Long running job queue (manageed by the lrj/jobs module) |
|ticket_acl| The network ACL used for issuing tickets |
|ticket_data *| The session data associated with a ticket managed by the ticket module. |
|tickets *| The ticket store |
|users| The users, used for authentication and managed with the apiuser modules. |

* *These collections are manageed by the API or its moudle code*

## Database Template

```javascript
//
// Create Users 
// The default admin user is 'super' and the password is 'youShouldChangeThis'
// The BFF server uses bffsrv and the same password.
//
db.users.insertMany([
  {
  	'_id': '98710bba-c2fe-451c-ba3b-74ed5e055898',
  	'user_id': 'super',
  	'password_hash': '$6$rounds=656000$Eqp8GQXSfNS6w5bh$YU6jTpYaTYjgWwfUkAjOf/3/21PaqfcL.nr7O9zhu/ivRomK/0vrnzkuT0WRSY9.9FEb6sX.Nj3AFfjav1f0p/',
  	'roles': ['super_user']
  },
  {
    '_id': 'edbd8a79-5147-44db-9057-722055d69fd0',
    'user_id': 'bffsrv',
    'password_hash': '$6$rounds=656000$Eqp8GQXSfNS6w5bh$YU6jTpYaTYjgWwfUkAjOf/3/21PaqfcL.nr7O9zhu/ivRomK/0vrnzkuT0WRSY9.9FEb6sX.Nj3AFfjav1f0p/',
    'roles': ['']
  }
];

// create ticket acl
db.ticket_acl.insertOne( {
    _id: '274ad0e9-9835-4e60-834b-99adda06583d',
    allow: [ '100.64.0.0/10' ],
    deny: [ '0.0.0.0/0' ],
    policy: 'deny'

});

// create bff hosts 
db.bff_hosts.insertMany([
  {
    "_id": "14aafd47-d42a-4c5b-b26e-63a58736626b",
    "node": "nodea",
    "interval": 1,
    "url": "https://nodea.taild0af6d.ts.net:9192"
  },
  {
    "_id": "25bbge58-e53b-5d6c-c37f-74b69847737c",
    "node": "nodeb",
    "interval": 1,
    "url": "https://debdns.taild0af6d.ts.net:9192"
  }
]);

db.host_info.insertMany(
  [
    {
    	'_id': 'c21695f0-04ec-47ec-b6c8-341ca011726e',
    	'class': 'upgrade_master',
    	'node': 'gitea',
      'dns': 'gitea'
  },
  {
      '_id': '12594ef3-86d7-45f1-b8b3-fee30e420ce3',
      'class': 'pvehost',
      'node': 'pve',
      'dns': 'pvehp.domain.tld'
  }]
);
// Create TTL indexes for expiring data
db.tickets.createIndex(
  { "createdAt": 1 }, 
  { expireAfterSeconds: 600 }
);

db.ticket_data.createIndex(
  { "createdAt": 1 }, 
  { expireAfterSeconds: 600 }
);

db.lrj_queue.createIndex(
  { "createdAt": 1 }, 
  { expireAfterSeconds: 3600 }
);
```