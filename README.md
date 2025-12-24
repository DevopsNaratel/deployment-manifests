# ☸️ ArgoCD Apps Deployment

Repositori ini berisi manifest Kubernetes (YAML) untuk berbagai aplikasi yang dikelola secara otomatis menggunakan **ArgoCD**. Karena ArgoCD sudah terpasang di cluster, repositori ini berfungsi sebagai sumber sinkronisasi utama.

## 📂 Struktur Direktori

Struktur repositori ini dirancang agar sederhana dan mudah dikembangkan:

```text
.
├── apps/                   # Seluruh manifest aplikasi ada di sini
│   ├── aplikasi-web-1/     # Contoh aplikasi web
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── ingress.yaml
│   │   └── secret.yaml
│   ├── aplikasi-web-2/     
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── ingress.yaml
│   │   └── secret.yaml
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

Anda bisa menambahkan aplikasi melalui UI ArgoCD atau dengan menerapkan manifest berikut menggunakan `kubectl`:

```yaml
# Simpan sebagai my-app-config.yaml lalu: kubectl apply -f my-app-config.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: nama-aplikasi-anda
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/username/nama-repo.git'
    targetRevision: HEAD
    path: apps/nama-folder-app  # Sesuaikan dengan folder di repo ini
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: default          # Namespace target di cluster
  syncPolicy:
    automated:                  # Sinkronisasi otomatis saat ada git push
      prune: true               # Hapus resource di k8s jika dihapus di git
      selfHeal: true            # Perbaiki jika ada perubahan manual di cluster

```

---

## 🔄 Alur Kerja (Workflow)

1. **Modify**: Lakukan perubahan pada file YAML di dalam folder `apps/`.
2. **Commit & Push**: Push perubahan ke branch `main`.
3. **Sync**: ArgoCD akan mendeteksi perubahan dalam waktu < 3 menit dan langsung melakukan *deployment* ke cluster.

---

## 🛠 Perintah Berguna

Jika Anda memiliki akses **ArgoCD CLI**, perintah berikut akan sering digunakan:

* **Melihat daftar aplikasi:**
`argocd app list`
* **Sinkronisasi manual (jika auto-sync mati):**
`argocd app sync <nama-app>`
* **Melihat status kesehatan aplikasi:**
`argocd app get <nama-app>`

---
