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

# copy package.json from the playwright service folder
COPY apps/playwright-service-ts/package*.json ./

RUN npm install

# install playwright browsers
RUN npx playwright install --with-deps chromium

# copy the rest of the service
COPY apps/playwright-service-ts .

RUN npm run build

EXPOSE 3000

CMD ["node", "dist/api.js"]
