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

FROM node:20-slim

WORKDIR /app

COPY package*.json ./

RUN npm install

# Install Playwright browsers
RUN npx playwright install --with-deps chromium

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["node", "dist/api.js"]
