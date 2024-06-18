# Ingress Nginx

> [Ingress-Nginx Controller](https://kubernetes.github.io/ingress-nginx/)

k3s 缺省安装 traefik 适合大部分场景。如果更习惯 Nginx，可以使用下面方法替代 Traefik.

K8S 支持多种 Ingress 并存，但是如何要把 Nginx 作为缺省 Ingress，只能关闭 Traefik.

- 关闭缺省的 traefik

  ```sh
  pdsh -w ^server "sed -i '/disable:/a - traefik' /etc/rancher/k3s/config.yaml"
  pdsh -w ^server systemctl restart k3s
  ```

- 部署 ingress nginx

  ```sh
  helmwave up --build
  ```

  其中 values 设置：

  - 把 Nginx 作为缺省 Ingress
  - 从 Service Load Balancer 限定申请的 IP，防止意外情况变化（比如删除再创建，这种可能申请的 IP 会变化）