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

FROM node:18-slim

WORKDIR /app

# Copy package files from correct directory
COPY apps/playwright-service-ts/package*.json ./

RUN npm install

# Copy service source
COPY apps/playwright-service-ts .

# Install Playwright browser
RUN npx playwright install chromium --with-deps

RUN npm run build

EXPOSE 3000

CMD ["node", "dist/api.js"]
