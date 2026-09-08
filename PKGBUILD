# Maintainer: Capricornus007 (auto-build via Capricornus007/repo)
# u1s1（有一说一）官方 TUI CLI：自帶 Node 22 運行時的便攜 tarball。
# 上游不提供單獨的構建源碼，AUR 慣例對這種「官方預編譯 TUI/CLI」用 -bin 後綴。
# 版本來源 https://u1s1.io/releases/LATEST；tarball 與 SHA256SUMS 由官方提供。
pkgname=u1s1-bin
pkgver=1.9.0
pkgrel=1
pkgdesc="u1s1 — 有一说一，说人话的 AI 编程搭子 (official TUI CLI with bundled Node.js runtime)"
arch=('x86_64' 'aarch64')
url="https://u1s1.io"
license=('LicenseRef-Proprietary')
depends=('bash')
optdepends=(
  'git: 讓 u1s1 在項目裡工作'
)
provides=('u1s1')
conflicts=('u1s1')
options=(!strip)
_base_url="https://u1s1.io/releases"
# 平台映射沿用官方 install.sh：x86_64→linux-x64、aarch64→linux-arm64
source_x86_64=("${_base_url}/u1s1-cli-${pkgver}-linux-x64.tar.gz")
source_aarch64=("${_base_url}/u1s1-cli-${pkgver}-linux-arm64.tar.gz")
# 校驗和按平台分開寫死；升版時由 CI 的 updpkgsums 更新
sha256sums_x86_64=('f441c44ea0b591f519ccee18c9e613da8b768fcbf8d6005c97478c0751434c8e')
sha256sums_aarch64=('SKIP')

# LATEST 是純文字版本號，CI 的 plan 步驟直接 curl 它做版本比對。
# 「更新校验和并回推 PKGBUILD」會用 updpkgsums 重算 x86_64 的校驗和回推；
# arm64 的 SKIP 只影響本地手動構建 arm64 的場景（倉庫只出 x86_64 包）。

package() {
  # 便攜包結構：u1s1（bash wrapper）→ node/bin/node → u1s1-cli/dist/index.js，
  # 三者相對路徑固定，整棵樹原樣放進 /opt/u1s1，再把 wrapper 掛到 PATH。
  # tarball 內含單一目錄 u1s1/（node、u1s1-cli、u1s1 wrapper 都在裡面）
  _src="$srcdir/u1s1"
  install -dm755 "$pkgdir/opt/u1s1"
  cp -a "$_src/node" "$_src/u1s1-cli" "$pkgdir/opt/u1s1/"
  install -Dm755 "$_src/u1s1" "$pkgdir/opt/u1s1/u1s1"
  # exec 轉發式 wrapper（$0 解析固定在 /opt/u1s1，與 symlink 行為無關）
  install -dm755 "$pkgdir/usr/bin"
  printf '#!/usr/bin/env bash\nexec /opt/u1s1/u1s1 "$@"\n' > "$pkgdir/usr/bin/u1s1"
  chmod 755 "$pkgdir/usr/bin/u1s1"
}
