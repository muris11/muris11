# Task 2 Report — Workflow Hardening

Plan file `docs/superpowers/plans/2026-09-16-premium-profile-readme.md` tidak ditemukan di worktree ini; dikerjakan langsung dari instruksi task.

## Analisis overlap
- `snake.yml` dan `update-readme.yml` sama-sama generate snake animation (duplikat, trigger sama: cron `0 */6 * * *` + push ke `main`). `update-readme.yml` tidak dihapus karena masih punya fungsi unik (waka-time stats), tapi step "Generate Snake Animation" di dalamnya dihapus — snake generation sekarang cuma dimiliki `snake.yml`.
- Trigger `push: branches: [main]` di `update-readme.yml` juga dihapus supaya tidak race/commit-conflict dengan `blog-post-workflow.yml`, `metrics.yml`, dan `snake.yml` yang juga bisa jalan di sekitar push ke main. `update-readme.yml` sekarang murni cron 6-jam + manual dispatch.
- `blog-post-workflow.yml` (hourly), `metrics.yml` (daily 00:00), `snake.yml` (6-jam + push main), `update-readme.yml` (6-jam) — jadwal tidak lagi saling menimpa README/snake yang sama.

## Perubahan per file
- **blog-post-workflow.yml**: tambah `permissions: contents: write`, `concurrency` group per-workflow.
- **metrics.yml**: tambah `permissions`/`concurrency`; pin `lowlighter/metrics@latest` → `@v3.34` (tag rilis stabil, verified via GitHub API tags).
- **snake.yml**: tambah `permissions`/`concurrency` (dibutuhkan untuk push branch `output`).
- **update-readme.yml**: tambah `permissions`/`concurrency`; pin `anmol098/waka-readme-stats@master` → `@v5` (verified via GitHub API tags); hapus step Snake Animation (redundant) dan trigger `push`.

Action lain (`actions/checkout@v3`, `Platane/snk/svg-only@v3`, `crazy-max/ghaction-github-pages@v3`, `gautamkrishnar/blog-post-workflow@v1`, `stefanzweifel/git-auto-commit-action@v4`) sudah pakai tag versi stabil (bukan `@master`/`@latest`), tidak diubah.

## Validasi
Semua 4 file divalidasi lolos parse via `npx js-yaml <file>` (tidak ada Python di environment).

## Concern
- `README.md` tidak disentuh sesuai instruksi.
- Tag `v3.34` (metrics) dan `v5` (waka-readme-stats) dipilih dari rilis GitHub terbaru saat ini — perlu di-review berkala kalau upstream rilis breaking major version baru.
- Tidak menambah `id-token`/scope lain karena tidak dipakai action manapun di sini — `contents: write` sudah cukup least-privilege untuk semua 4 workflow (checkout hanya butuh read, tapi write diberikan di level job karena tiap job melakukan commit/push).
