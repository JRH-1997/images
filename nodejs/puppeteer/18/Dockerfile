FROM        node:18
LABEL       author="Michael Parker" maintainer="parker@pterodactyl.io"

RUN         apt update \
            && apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev ca-certificates dnsutils tzdata zip tar curl build-essential wget gnupg \
            && npm -g install npm@latest \
            && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmor -o /usr/share/keyrings/googlechrome-linux-keyring.gpg \
    		&& sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/googlechrome-linux-keyring.gpg] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    		&& apt update \
    		&& apt install -y google-chrome-stable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-khmeros fonts-kacst fonts-freefont-ttf libxss1 \
      			--no-install-recommends \
    		&& rm -rf /var/lib/apt/lists/* \
    		&& groupadd -r container && useradd -rm -g container -G audio,video container

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

# Install @puppeteer/browsers, puppeteer and puppeteer-core into /home/pptruser/node_modules.

# cd to the downloaded instance
		# copy sandbox executable to a shared location
# export CHROME_DEVEL_SANDBOX env variable

RUN npm i @puppeteer/browsers puppeteer puppeteer-core \
    	&& (node -e "require('child_process').execSync(require('puppeteer').executablePath() + ' --credits', {stdio: 'inherit'})" > THIRD_PARTY_NOTICES) \
	 	&& ls \
	 	&& cd node_modulea \
		&& ls \
	  	&& cd puppeteer \
  		&& ls \
  		&& cd .local-chromium \
  		&& ls \
  		&& cd linux-* \
  		&& ls \
  		cd chrome-linux \
  		&& ls \  
		&& chown container:container chrome_sandbox \
  		&& chmod 4755 chrome_sandbox \
  		&& cp -p chrome_sandbox /home/container/sbin/chrome-devel-sandbox

ENV 	CHROME_DEVEL_SANDBOX=/home/container/sbin/chrome-devel-sandbox
 
COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
