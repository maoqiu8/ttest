FROM node:lts-alpine3.12

LABEL AUTHOR="none" \
      VERSION=0.1.4

ARG KEY="-----BEGIN OPENSSH PRIVATE KEY-----\nb3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABFwAAAAdzc2gtcn\nNhAAAAAwEAAQAAAQEAyqjKT5hR8W8GrGMe3q0GDphgCjyQSYwjucFFQZxGkvhEI4146Ija\nWLPthwKSgyixi/byqa/JSPBVEw4dx2zcNPKfCg4Hy/hzTgRTW44gBTcJ/hX6g+ODHUY6ip\ntm7crORqQ3AKBMVbna6DrAh6GYpwVYCzSfr6Gls37mDIV6BFmQJHucUk4RdCKg9jpFNed1\nCTKHPP/+YeW1bHWnKd9uMA91Y7cooDoZz8tePWjKY0bgoZZUjIvdEXvcIVM57njnEwEyC8\nX0YmqmNh155yiCT0wW78GgtWYkIvO6ShvhwH+53biGYgz5IH+BvCcHzvxoIPza0HtLuI/N\nkijfiXKScwAAA9ieqJGjnqiRowAAAAdzc2gtcnNhAAABAQDKqMpPmFHxbwasYx7erQYOmG\nAKPJBJjCO5wUVBnEaS+EQjjXjoiNpYs+2HApKDKLGL9vKpr8lI8FUTDh3HbNw08p8KDgfL\n+HNOBFNbjiAFNwn+FfqD44MdRjqKm2btys5GpDcAoExVudroOsCHoZinBVgLNJ+voaWzfu\nYMhXoEWZAke5xSThF0IqD2OkU153UJMoc8//5h5bVsdacp324wD3VjtyigOhnPy149aMpj\nRuChllSMi90Re9whUznueOcTATILxfRiaqY2HXnnKIJPTBbvwaC1ZiQi87pKG+HAf7nduI\nZiDPkgf4G8JwfO/Ggg/NrQe0u4j82SKN+JcpJzAAAAAwEAAQAAAQEAu55SVif9TNhtxDCr\nxplGr0JwP2ncZsqf1HvSYzCLwcqoQoXZ9uWjx1UkRMaj31tQK/T+qQu9/JHLIS4Dcbodj8\nNCa6ubmkE0dnjYtz039bwDu0CbwXhizTpUPjgqrWEBYTLsXN8KbJ74w+CIl8iKK65t2Unp\nwE6Vujx0PAlichzUfNPNPC7nOChgo45N18843Hs2BUIF7wRQ2g1dLA5d78eRrM+6F9Ia75\nNk7NBtAr5oVmAVj9PgZVfJejUsPpjZYACMIf6CimrzMnD9BQupj+b6i/e/Rdt2Xs9S+BOD\nZeuMbp4KfHBIbpGSvl6HY/HYN/5Ns9CPKpS4sfbcaOJimQAAAIEAoOyhmCaJsOv2gy99gu\npWCNOabBW9W4zql0O4hNa+PuNreFsCR/4pQSoc9xbkEn9eKHQrKDFFGeVeTaxMyhmGTulL\nCCp5SbGSfGTCQh0Yp57uHtju+IQdeoeV9xVGvzsoRKmgqRFXs2VIIaVvQzaWs3NeFubCHe\no60G7ZLfSUnUkAAACBAP2vRIWv/FkXwXdGV0GJsAlX6fEqpSCMOuzbunQgF96wZvCJNXmk\niiAVK7YZyQKCi2WAPOsV6dbwPdQrqa8eXOsDAkut1PLCsL0q+S9LvXBknsv7Spoh6q6n/V\nXVTO/GuGu1kNskM0SwgGVb5TPC+Y8THPHOO1APgeMaR6ckGdE3AAAAgQDMgk1nfDA/Fp1N\nTG7WXW7Fd/nly00TscBE1gU0pN0B6fQ3WeHtVHyeZPCsqD2gg9Iaz+W43yte9oD7/UeXsy\nORH7rd3e6xH5qYlD1vzxKfIrhaoDQX4ZvR/g8DYEMrTVJaJthPpn7HUumf6PIa2pfdplWF\ndzNTOlsyNxs408wWpQAAABp3aXNkb20yNjIyMDY5MjUyQGdtYWlsLmNvbQECAwQFBgc=\n-----END OPENSSH PRIVATE KEY-----"

ENV DEFAULT_LIST_FILE=crontab_list.sh \
    CUSTOM_LIST_MERGE_TYPE=append \
    COOKIES_LIST=/scripts/logs/cookies.list \
    REPO_URL=git@gitee.com:wisdomclouds/jd_scripts.git \
    REPO_BRANCH=master

RUN set -ex \
    && apk update \
    && apk upgrade \
    && apk add --no-cache bash tzdata git moreutils curl jq openssh-client \
    && rm -rf /var/cache/apk/* \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && mkdir -p /root/.ssh \
    && echo -e $KEY > /root/.ssh/id_rsa \
    && chmod 600 /root/.ssh/id_rsa \
    && ssh-keyscan gitee.com > /root/.ssh/known_hosts \
    && git clone -b $REPO_BRANCH $REPO_URL /scripts \
    && cd /scripts \
    && mkdir logs \
    && npm config set registry https://registry.npm.taobao.org \
    && npm install \
    && cp /scripts/docker/docker_entrypoint.sh /usr/local/bin \
    && chmod +x /usr/local/bin/docker_entrypoint.sh

WORKDIR /scripts

ENTRYPOINT ["docker_entrypoint.sh"]

CMD [ "crond" ]
