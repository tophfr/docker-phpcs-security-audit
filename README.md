# Docker image for PHP Security Audit with PHP CodeSniffer

Cf. https://github.com/FloeDesignTechnologies/phpcs-security-audit

## How to use it

    docker run --rm -it --init -v "$PWD:$PWD" -w "$PWD" tophfr/phpcs-security-audit -p .

