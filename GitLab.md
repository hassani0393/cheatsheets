#GitLab

Setup SSL for Gitlab

    openssl genrsa -out key.pem 2048

    openssl req -new -sha256 -key key.pem -out csr.csr

    