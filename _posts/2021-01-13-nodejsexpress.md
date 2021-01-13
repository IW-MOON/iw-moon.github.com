---
title: Node.js(Express) 구조
categories: []
---

### **Routes 구조**
---
> (1) source/app.js 

![app1](https://user-images.githubusercontent.com/72685070/104446838-5a3d4e80-55de-11eb-9de6-01285eb26b9c.PNG)
```javascript
app.use('/', indexRouter)
```

> (2) source/routes/index.js

![app2](https://user-images.githubusercontent.com/72685070/104446896-6d501e80-55de-11eb-8eed-04aa4fd18d45.PNG)
```javascript
const login = require('./login/index');
const upload = require('./upload/index');
const user = require('./user/index');
const workout = require('./workout/index');
```

> (3) source/routes/workout/index.js

![app3](https://user-images.githubusercontent.com/72685070/104447643-6bd32600-55df-11eb-91ff-7fd0e1464def.PNG)

```javascript
const express = require('express');
const router = express.Router();
const controller = require('./workout.controller');

router.get('/my', controller.getMyWcRt);
router.get('/my/:my_wc_idx', controller.getMyWcIdxRt);
router.post('/my/idx', controller.postMyWcIdxRt);
router.post('/my/alarm', controller.postMyWcAlarmRt);
router.get('/check/:type/:date', controller.getMyWcCheckRt);
module.exports = router;
```

> (4) source/routes/workout/workout.controller.js

```javascript
const common = require('../common');
const dao = require('./workout.dao');
const auth_config_dev = require('/source/config/auth_config_dev.js');
const moment = require('moment');

exports.getMyWcRt = (req, res, next) => {

}

```

> (5) source/routes/workout/workout.dao.js

```javascript
const getConnection = require('/source/config/db_config_dev.js');
const common = require('../common');
const mybatisMapper = require('mybatis-mapper');
mybatisMapper.createMapper([ './routes/workout/workout.mapper.xml' ]);
const format = {language: 'sql', indent: '  '};

module.exports = {

    getMyWc :  function(user_idx){
        console.log(user_idx);
        var params = {
            user_idx : user_idx,
            use_yn : 'y'
        };
        var query = mybatisMapper.getStatement('workoutMapper', 'selectMyWc', params, format);
        return common.returnSelectPromise(query);
    },

}

```
