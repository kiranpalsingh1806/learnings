# Deployment

- [Deployment](#deployment)
  - [1. Using PM2 Ecosystem File](#1-using-pm2-ecosystem-file)


## 1. Using PM2 Ecosystem File

```js
module.exports = {
    apps: [{
      name: 'Express API - 1',
      script: 'server.js',
      args: 'EXPRESS REST API - 1',
      instances: 1,
      autorestart: true,
      watch: false,
      max_memory_restart: '1G',
      env: {
        NODE_ENV: 'development',
        PORT: 4000
      }
    },
    {
      name: 'Express API - 2',
      script: 'server.js',
      args: 'EXPRESS REST API - 2',
      instances: 1,
      autorestart: true,
      watch: false,
      max_memory_restart: '1G',
      env: {
        NODE_ENV: 'development',
        PORT: 4001
      }
    }]
};
```

```sh
# Start all applications
pm2 start ecosystem.config.js

# Stop all
pm2 stop ecosystem.config.js

# Restart all
pm2 restart ecosystem.config.js

# Reload all
pm2 reload ecosystem.config.js

# Delete all
pm2 delete ecosystem.config.js
```