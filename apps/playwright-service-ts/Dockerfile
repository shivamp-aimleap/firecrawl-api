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

# install dependencies required by chromium
RUN apt-get update && apt-get install -y \
    wget \
    gnupg \
    ca-certificates \
    fonts-liberation \
    libnss3 \
    libatk-bridge2.0-0 \
    libatk1.0-0 \
    libcups2 \
    libxkbcommon0 \
    libxcomposite1 \
    libxdamage1 \
    libxrandr2 \
    libgbm1 \
    libasound2 \
    libpangocairo-1.0-0 \
    libgtk-3-0 \
    && rm -rf /var/lib/apt/lists/*

# copy package files
COPY apps/playwright-service-ts/package*.json ./

RUN npm install

# install browsers
RUN npx playwright install chromium

# copy source
COPY apps/playwright-service-ts .

RUN npm run build

EXPOSE 3000

CMD ["node", "dist/api.js"]
