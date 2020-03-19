# ApiJson Node版

> ### A ApiJson Server build with Nest.js and TypeORM

----------

# 测试环境

添加hosts
```
# /etc/hosts
192.168.94.186 apijson.guahao-test.com
```
测试环境地址 http://apijson.guahao-test.com

# 起步

## 安装依赖

    
```bash
$ npm install
```

----------

## Database数据库配置

修改 `ormconfig.js` 参照 `ormconfig.js.example`

    {
      "type": "mysql",
      "host": "localhost",
      "port": 3306,
      "username": "",
      "password": "",
      "database": "",
      "entities": ["src/**/**.entity{.ts,.js}"],
      "synchronize": false
    }

----------

## NPM 命令

- `npm run dev` - 开发模式
- `npm start` - 启动应用
- `npm run start:prod` - 生产模式

----------

## 启动应用

- `npm start`
- 健康检查 `http://localhost:3000/test` 正常情况下会返回 `ok`

----------
 
# Swagger API 文档

This example repo uses the NestJS swagger module for API documentation. [NestJS Swagger](https://github.com/nestjs/swagger) - [www.swagger.io](https://swagger.io/)        

# 使用文档

> 以下例子中 默认存在两张表（Comment， User）


已经实现的操作符

- 单条记录查询

  ```json
  {
      "Comment": {
        "User": {}
      }
  }
  ```

- []

  操作符名称： 列表查询

  例子：

    ```
      {
        "XXX[]": {  // 这里[]前的字符串将作为response的字段名
          "Comment": {}
        }
      }
    ```

- \#

  操作符名称： 别名

  例子：

    ```json
    {
      "SingleComment#": {
        "Comment": {}
      }
    }
    ```

- @column

  操作符名称： 字段筛选

  例子：

  ```json
  {
    "User": {
      "@column": "role,nickname"
    }
  }
  ```

- %
  
  操作符名称： DateRange

  例子：
  ```
  {
    "User": {
      "createdAt%": "2019-11-01,"   // 表示 2019-11-01至今
                                    // ",2019-11-01" 表示2019-11-01之前
                                    // "2019-11-01, 2019-11-02" 时间范围
                                    // "2019-11-01 00:00, 2019-11-01 14:00" 小时分钟也是支持的
                                    // 只要 时间字符串 可以被  `Date.parse(timeString)` 解析 都是可以通过的
    }
  }
  ```


- 联表查询

  例子：
  ```
  // 查询叫tony的User
  // 查询一条userId为User中id的Comment
  {
    "User": {
      "user": "tony"
    },
    "Comment": {
      "userId@": "User/id"
    }
  }

  // 查询所有符合条件的comment 显示 第1页 每页2条
  // (因为默认page = 1 count = 10 所以默认最多为10条)
  {
    "User": {
      "user": "tony"
    },
    "msgList[]": {
    	"Comment": {
        "userId@": "User/id"
	    },
	    "count": 2,
	    "page": 1
    }
  }
  
  ```


- 综合例子

```json
{
  "userInfo#": {
    "User": {
      "user": "tony"
    }
  },
  "testAlias#": {
    "msgList[]": {
      "Comment": {
        "userId@": "userInfo#/User/id"
      },
      "count": 2,
      "page": 1
    }
  }
}
```
