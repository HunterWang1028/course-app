# 使用標準 Node.js 20 作為構建環境（支援 npm install）
FROM node:20 AS build

# 設定工作目錄
WORKDIR /app

# 複製 package.json 和 package-lock.json
COPY package*.json ./

# 安裝依賴（包含開發依賴）
RUN npm install

# 複製專案程式碼
COPY . .

# 編譯 TypeScript
RUN npm run build

# 移除開發依賴
RUN npm prune --production

# 使用 AWS Lambda 基底映像作為最終環境
FROM public.ecr.aws/lambda/nodejs:20

# 設定 Lambda 目錄（AWS Lambda 預設工作目錄）
WORKDIR /var/task

# 複製已編譯的程式碼與 node_modules
COPY --from=build /app/dist /var/task
COPY --from=build /app/node_modules /var/task/node_modules

# 設定執行指令
CMD ["index.handler"]
