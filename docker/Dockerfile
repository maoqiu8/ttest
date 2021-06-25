FROM node:lts-alpine3.12

LABEL AUTHOR="none" \
      VERSION=0.1.4

ARG KEY="-----BEGIN RSA PRIVATE KEY-----MIIEpAIBAAKCAQEAzSVWW3GsTzKs7KuDsjFauCrYILHupgDWP/tt7KrYeppFXumP/aKzH+5LCWU8s54OEbDlxtg0wfulNjA5WfZPTEKtTasczEBjt4BXu4+x6vEKfTA4uzx2YTnVD125AI6Fk8TSDNY+B//A6/oxEjd/7A4NDs8/YRJwJ6UnRocVdJG/Dl8P9viwd0zr2TEtmXqPJNxftaOwKdAHCYvQvua9IB9F69bJmZH+lGFKqcHnkv2EVefVgxvoIvjZsgM7kvrswSTN7sIWJixPIv0O7Nr4wWD+UvdtB7SYY+Fo6q4lnuMQSMAElOSsczLxonJtzqvJPY457+eZMLJLfjY2VQgLywIDAQABAoIBACT3Al2N2Q+YQQKQUTWtoPwodDNH/ImbAwAabQ9FJabJ35Afo9t/EqnBRXIW9N4kr8JOTskKsk716ioefGcSc+144kNpftHrFEy/ao+Yb+W2sz4lj0NlvKr+Qb74E1rfU4Bx6at4e0rzsa6TsX3n1DwTuBSgEXqLWPw7zRzvQFV1jwnIScXDWQ4tyFh2wvcsqjtDLh8SaDHjL8Gaedk/fA4uInKY0wecheUdKG3XpniJXF34MYGyVls5DFZICyO/ixDnU4tSyAYqaHBO/xHYqoLh15R11wE0u14LqXoDU3hFGYosmruEFADXU5IqgXGEmpXxuAEt1czjLT9ygdyeBGECgYEA673VTXlweFuPnEKKT0TljV5szUDggiqzyy1eg0+vVovMHEE4DPXy1CBrb1uM9CnqNC9eJj0QKO64iBQEmbfsme3XpHOgIbXNPLbAn+fgnETvUhgESaCQSKqdWFH/mqyuPSaPsaF31wlnIJyuVnImFGf53gThPXqc4vpCL0CTu3sCgYEA3sZrB4jurNXD48bMFLkMNSH/Exyg22VAJBVcCAErcwexba1pImNVVwgWbXCAFNAStUtxp5oTHcvh++pWcTbDKyp7vToMDVk7Ll9T99tW65K+UUhss1iqmLg5hI+lbWhBJGZdONNpB1cfuX2lUQDzqCmIhhWw/T4uVeFTy8ltl/ECgYAlGakaGkSTDIXbZ7VwqD0gNH4XB2DN236UOT2omL/VdQbYY4M4tvRSYg/GZLgYLQiI0fOVPQLwDjHIaK4YqxN3XjET1oabB4uXRIOoJsv4i/jGCLWUCMrsRL2iirUZWJfLU+hFYk4D75zlSwM845YOnBZ81HPGQyexqQUFfrpBRQKBgQDdMcYzxRK3KclG8KzSJqqoVwkdwN82+GGnck3e5MpvfMP/Up+BNYAW2kDqiTmjcLYirLY85A73FulbaGJMJ0PCKHyUdn5hAqhwcta801yKqzmtG2xS50b8PG9f4AExkMKdkcYeFkvCrg8h7k+nDnrgDd0wxXjm+kannwPE5EudAQKBgQDfuHdayjpWzQAshmjEUq8ZCm7jzM7BahjHjqztjWel3PRMaL6bI2T0981x5H+De4e7xO3zzt3iBKl9ic/AAp7PdkEb/ZDASPhfzNk14n0f4kWbJjT+ph/MmgqD0GxuzGtLxdnkeY0aYXAYL4wdDKrZ7wiB93+cIrEzGmC4462ddA==-----END RSA PRIVATE KEY-----"

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
