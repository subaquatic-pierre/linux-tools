# Apollo Client - Server config settings

General settings to use when configuring ApolloServer and ApolloClient with Nginx

## Nginx configurations

#### /etc/nginx/conf.d/webserver

    server {
        listen 80;
        listen [::]:80;
        server_name www.example.com example.com;
        return 301 https://example.com$request_uri;
    }

    server {
        listen 80;
        server_name www.example.com example.com

        location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 2;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }

        # Forward graphql to running server
        location /graphql {
            proxy_pass http://localhost:4000/graphql;
        }

    }

    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

#### /myapp/client/index.js

    // create link to server
    const httpLink = new HttpLink({ uri: '/graphql' });

    // create new apollo client
    const client = new ApolloClient({

        // set link to ApolloLink and httpLink
        link: authLink.concat(httpLink),
        cache: new InMemoryCache(),
        onError: (error) => {
            console.log(error)
        }

    });

#### /myapp/client/index.js

    // Import Apollo server from serverConfig.js
    import server from './serverConfig.js'

    // Variables should be imported from secret location
    const DB_URI = mongodb://127.0.0.1:27017/example_app
    const USER = 'Cool'
    const PASSWORD = 'supersecret'

    // connect to database from config variables
    mongoose.connect(DB_URI, {
        useNewUrlParser: true,
        useUnifiedTopology: true,
        useCreateIndex: true,
        user: USER,
        pass: PASSWORD
    }).then(() => console.log(`Connected to database at ${DB_URI}`)).catch(err => console.log(err));

    server.listen().then(() => {
        console.log('🚀  Server ready at http://localhost:4000/')
    })
