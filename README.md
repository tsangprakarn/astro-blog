# Astro Blog Template for Home K8s (ghcr.io version)

## วิธีใช้แบบง่ายสุดสำหรับสาย Hobby
1. npm create astro@latest . -- --template blog --no-git --no-install
   npm install
2. เอา 4 ไฟล์นี้ไปวางทับที่ root ของโปรเจค Astro ที่สร้างมา
   - Dockerfile
   - nginx.conf
   - k8s/deployment.yaml (แก้ image เป็น ghcr.io/USERNAME/astro-blog:latest)
   - .github/workflows/build.yaml

3. git init, git add ., git commit, git push ไป repo ชื่อ astro-blog

4. ไปที่ GitHub > Packages > astro-blog > Settings > Change visibility -> Public

5. ที่ K8s Master ที่บ้าน:
   kubectl apply -f k8s/deployment.yaml

6. เพิ่มใน HAProxy 10.8.0.5:
   frontend http-in
     acl is_blog hdr(host) -i blog.my.domain
     use_backend k8s_astro_blog if is_blog
   backend k8s_astro_blog
     server master 192.168.1.50:30080 check

7. เขียนบล็อกใหม่ใน src/content/blog/ แล้ว git push
   แล้วที่บ้าน: kubectl rollout restart deployment astro-blog
