name: CI/CD Pipeline - Azure WebApp

on:
  push:
    branches:
      - main   # Trigger on push to main branch

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout source code
      uses: actions/checkout@v4

    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '18'

    - name: Install dependencies
      run: npm install

    - name: Build project
      run: npm run build

    - name: Upload artifact for deployment
      uses: actions/upload-artifact@v4
      with:
        name: webapp
        path: ./build

    - name: Deploy to Azure WebApp
      uses: azure/webapps-deploy@v2
      with:
        app-name: your-azure-webapp-name   # Replace with your WebApp name
        slot-name: production
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        package: ./build
