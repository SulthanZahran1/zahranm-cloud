FROM nginx:alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY index.html index.md site.css robots.txt llms.txt agent-instructions.md sitemap.xml sitemap.md 404.md og.svg /usr/share/nginx/html/
COPY about /usr/share/nginx/html/about
COPY contact /usr/share/nginx/html/contact
COPY privacy /usr/share/nginx/html/privacy
EXPOSE 80
