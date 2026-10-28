# Real-Time Chat

A real-time group chat built with **Laravel**, **Laravel Echo**, **Socket.io** and **Redis**. Messages are broadcast instantly to everyone in the room, and a presence channel shows who is online.

![Laravel](https://img.shields.io/badge/Laravel-5.8-FF2D20?logo=laravel&logoColor=white)
![Socket.io](https://img.shields.io/badge/Socket.io-010101?logo=socketdotio&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?logo=mysql&logoColor=white)

## Features

- User registration and login
- Group chat room: messages are saved and broadcast to other users in real time
- **Online users list** using a presence channel (`here`, `joining`, `leaving`)
- Broadcasting with a `MessageDeliverd` event sent `toOthers()`
- Self-hosted WebSocket server with **laravel-echo-server** and Redis (no Pusher required)

## How It Works

```
Browser ──POST /messages──> Laravel ──event──> Redis
                                                 │
Browser <──Socket.io── laravel-echo-server <─────┘
```

1. A user sends a message; `MessageController@store` saves it and broadcasts `MessageDeliverd`.
2. Laravel publishes the event to **Redis**.
3. **laravel-echo-server** reads it from Redis and pushes it over Socket.io.
4. **Laravel Echo** in the browser listens on the `chat-group` channel and the `online` presence channel.

## Getting Started

**Prerequisites:** PHP 7.1.3+, Composer, Node.js, MySQL, Redis

```bash
git clone https://github.com/mostfaswaisi93/chat-app.git
cd chat-app
composer install
npm install && npm run dev
npm install -g laravel-echo-server

cp .env.example .env
php artisan key:generate
# set DB_* values, BROADCAST_DRIVER=redis and REDIS_* values in .env
php artisan migrate

php artisan serve          # http://localhost:8000
laravel-echo-server start  # WebSocket server (config in laravel-echo-server.json)
php artisan queue:work     # if broadcasting is queued
```

## Author

**Mustafa T. Al-Swaisi** · [LinkedIn](https://www.linkedin.com/in/mostfaswaisi93/) · [GitHub](https://github.com/mostfaswaisi93)
