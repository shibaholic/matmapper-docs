# Frontend setup

Setting up the frontend Vite + React + TS for fast HMR.

## Setup

### Install steps

Removed old Node. Installed LTS Node.

`node -v` : v20.17.0

`npm -v` : 10.8.2

Install yarn. I will use yarn from now on.

`sudo npm install yarn -g`

`yarn -v` : 1.22.22

Created project using vite.

`yarn create vite client --template react-ts`

Had to also install vite manually for some reason.

`yarn add -D vite`

### Setup Bootstrap

`yarn add react-bootstrap bootstrap`

`yarn add @types/bootstrap`

### Setup React Router

`yarn add react-router-dom`

### Add react-window

`yarn add react-window`

`yarn add @types/react-window`

`yarn add react-virtualized-auto-sizer`

### Setup SignalR

`yarn add @microsoft/signalr`

### Setup diff

`yarn add diff`

`yarn add @types/diff`

## Local development

`yarn run dev`

Local: http://localhost:5173

On local network (expose with `--host`): http://192.168.86.100:5173

## package.json Scripts
```json
"dev": "vite",
"build": "tsc -b && vite build",
"lint": "eslint .",
"preview": "vite preview"
```

Remove `tsc -b` to disable Typescript warnings that block publishing (such as "this variable was not used").

For example: `yarn run dev`.