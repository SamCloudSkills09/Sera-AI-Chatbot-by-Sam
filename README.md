cd sera-chatbot
git init
node_modules/
build/
.env
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/your-username/sera-chatbot.git
git push -u origin master
npm install gh-pages --save-dev
"scripts": {
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build"
}
npm run deploy
