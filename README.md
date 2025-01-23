# oly_crud_demo

## File Structure

### `src/`
Source code for the project.

- **`components/`**: Contains the components used in the project.
  - **`ItemForm.vue`**: Component for adding new items.
  - **`ItemList.vue`**: Component for displaying the list of items.
  - **`ItemEdit.vue`**: Component for editing an existing item.
  - **`ConfirmDialog.vue`**: Component for displaying a confirmation dialog.
  
- **`App.vue`**: Main component.

A simple web app built with vuejs that supports note functionality. When the user enters a phone number, a request will be sent to abstract API and will return some basic data about the carrier/location and country etc.

## API Keys
Get your API Key from https://docs.abstractapi.com/phone-validation?_gl=1*1my33iv*_gcl_au*MTUyMDY5Mzc0OC4xNzM3NTk1MTY3

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
serve -s dist -l tcp://0.0.0.0:5000
```
