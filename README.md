install sequelize and helper libraries, along with dotenv library
`npm i sequelize pg pg-hstore dotenv`

Set up a CONNECTION_STRING variable in your .env file

CONNECTION_STRING=[your postgres db uri]

setup sequelize in controller or other server file:
-require and configure .env process, import environmental variable
-require Sequelize class
-create sequelize instance

```
require('dotenv').config()

const {CONNECTION_STRING} = process.env

const Sequelize = require('sequelize')

const sequelize = new Sequelize(CONNECTION_STRING, {
    dialect: 'postgres',
    dialectOptions: {
        ssl: {
            rejectUnauthorized: false
        }
    }
})

```

sequelize instance can now be used in handler to create query such as:

```
module.exports = {
    getUserInfo: (req, res) => {
        sequelize.query(`
        SELECT * 
        FROM cc_clients c
        JOIN cc_users u
        ON c.user_id = u.user_id
        WHERE u.user_id = ${userId};`)
            .then(dbRes => res.status(200).send(dbRes[0]))
            .catch(err => console.log(err))
    },
    ```
