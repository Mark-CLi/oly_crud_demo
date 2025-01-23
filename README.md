# oly_crud_demo

## Pre-requisites
node
npm

you can verify the installation by running the following commands
```
node -v
npm -v
```
if not installed, perform the following commands
```
sudo apt-get update
sudo apt-get install nodejs
sudo apt-get install npm
```

## Download file from github
```
git clone https://github.com/Mark-CLi/oly_crud_demo.git
```

## Build the project
```
cd oly_crud_demo
npm install
npm run build
```

## Serve the project via Node.js
```
npm install -g serve
serve -s dist
serve -s dist -l 0.0.0.0:5000
```