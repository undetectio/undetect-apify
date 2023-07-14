ARG CONTAINER_KEY
FROM cr.undetect.io/$CONTAINER_KEY/stealthium:latest AS builder

COPY --chown=browser:root package*.json ./

RUN npm install --include=dev --audit=false

COPY --chown=browser:root . ./

RUN npm run build

FROM cr.undetect.io/$CONTAINER_KEY/stealthium:latest

COPY --from=builder --chown=browser:root /home/browser/dist ./dist

COPY --chown=browser:root package*.json ./

RUN npm --quiet set progress=false \
    && npm install --omit=dev --omit=optional \
    && echo "Installed NPM packages:" \
    && (npm list --omit=dev --all || true) \
    && echo "Node.js version:" \
    && node --version \
    && echo "NPM version:" \
    && npm --version \
    && rm -r ~/.npm

COPY --chown=browser:root . ./

ENV APIFY_DISABLE_OUTDATED_WARNING=1

ARG UNDETECT_KEY
ENV UNDETECT_KEY=$UNDETECT_KEY
ENV APIFY_CHROME_EXECUTABLE_PATH=/usr/bin/stealthium

CMD npm run start:prod --silent
