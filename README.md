# ☸️ ArgoCD Apps Deployment

Repositori ini berisi manifest Kubernetes (YAML) untuk berbagai aplikasi yang dikelola secara otomatis menggunakan **ArgoCD**. Karena ArgoCD sudah terpasang di cluster, repositori ini berfungsi sebagai sumber sinkronisasi utama.

## 📂 Struktur Direktori

Struktur repositori ini dirancang agar sederhana dan mudah dikembangkan:

```text
.
├── deploymen-manifests/     # Seluruh manifest aplikasi ada di sini
│   ├── aplikasi-web-1/      # Contoh aplikasi web
│   │   ├── deployment.yaml
│   │   └── ingress.yaml
│   ├── aplikasi-web-2/     
│   │   ├── deployment.yaml
│   │   └── ingress.yaml
└── README.md

```

---

## 🚀 Cara Menambahkan Aplikasi ke ArgoCD

Karena ArgoCD sudah terinstal, Anda hanya perlu mendaftarkan folder di dalam repo ini sebagai **Application** di ArgoCD.

### 1. Hubungkan Repo (Jika Belum)

Jika repositori ini bersifat *private*, tambahkan kredensial di UI ArgoCD atau via CLI:

```bash
argocd repo add https://github.com/username/nama-repo.git --username <user> --password <token>

```

### 2. Buat Aplikasi Baru

Anda bisa menambahkan aplikasi melalui UI ArgoCD 
```

---

## 🔄 Alur Kerja (Workflow)

1. **Modify**: Lakukan perubahan pada file YAML di dalam folder `apps/`.
2. **Commit & Push**: Push perubahan ke branch `main`.
3. **Sync**: ArgoCD akan mendeteksi perubahan dalam waktu < 3 menit dan langsung melakukan *deployment* ke cluster.

---
