# FROM node:18-slim

# WORKDIR /usr/src/app
# COPY package*.json ./
# RUN npm install

# COPY . .

# ENV PLAYWRIGHT_BROWSERS_PATH=/usr/local/share/playwright

# # Install Playwright dependencies
# RUN npx playwright install chromium --with-deps

# RUN npm run build

# ARG PORT
# ENV PORT=${PORT}

# EXPOSE ${PORT}

# CMD [ "npm", "start" ]

FROM mcr.microsoft.com/playwright:v1.43.0-jammy

WORKDIR /app

# copy package files
COPY apps/playwright-service-ts/package*.json ./

RUN npm install

# copy service code
COPY apps/playwright-service-ts .

RUN npm run build

EXPOSE 3000

CMD ["node", "dist/api.js"]
