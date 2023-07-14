ARG CONTAINER_KEY
FROM cr.undetect.io/$CONTAINER_KEY/stealthium:latest AS builder

COPY --chown=browser package*.json ./

RUN npm install --include=dev --audit=false

COPY --chown=browser . ./

RUN npm run build

FROM cr.undetect.io/$CONTAINER_KEY/stealthium:latest

COPY --from=builder --chown=browser /home/browser/dist ./dist

COPY --chown=browser package*.json ./

RUN npm --quiet set progress=false \
    && npm install --omit=dev --omit=optional \
    && echo "Installed NPM packages:" \
    && (npm list --omit=dev --all || true) \
    && echo "Node.js version:" \
    && node --version \
    && echo "NPM version:" \
    && npm --version \
    && rm -r ~/.npm

COPY --chown=browser . ./

ENV APIFY_DISABLE_OUTDATED_WARNING=1

CMD npm run start:prod --silent
