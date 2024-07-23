# Maintainer: Zdeněk Biberle <zdenek at biberle dot net>
pkgname=snx-rs
pkgver=2.2.6
pkgrel=1
pkgdesc="Rust client for Checkpoint VPN tunnels"
arch=(x86_64)
url=https://github.com/ancwrd1/snx-rs
license=(AGPL-3.0)
depends=(gcc-libs glibc openssl webkit2gtk-4.1)
makedepends=(cargo)
checkdepends=(iproute2)
source=(
  "$pkgname-$pkgver.tar.gz::https://github.com/ancwrd1/$pkgname/archive/refs/tags/v$pkgver.tar.gz"
  fix-executable-path.patch
)
sha256sums=('7b47230744b2d54155f6395d558aae2965d0119c31c5bfad200833f6ac44ae78'
            'c4438f1167b76cc278610faacdd6d821e21a9339dd12fd86bf5c27f6af66424d')

prepare() {
  cd "$pkgname-$pkgver"
  patch --forward --strip=1 --input="$srcdir/fix-executable-path.patch"
  export RUSTUP_TOOLCHAIN=stable
  cargo fetch --locked --target "$CARCH-unknown-linux-gnu"
}

build() {
  cd "$pkgname-$pkgver"
  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR=target
  cargo build --frozen --release
}

check() {
  cd "$pkgname-$pkgver"
  export RUSTUP_TOOLCHAIN=stable
  cargo test --frozen
}

package() {
  # At runtime, libappindicator-sys loads either libayatana-appindicator3.so
  # or libappindicator3.so to do its thing. These come from the
  # libayatana-appindicator and libappindicator-gtk3 packages.
  # Either one works, but as libappindicator-gtk3 seems to be on its way
  # out, I've chosen to depend on libayatana-appindicator.
  depends+=(libayatana-appindicator systemd iproute2)
  cd "$pkgname-$pkgver"
  install -Dm0755 -t "$pkgdir/usr/bin/" target/release/{snx-rs,snxctl,snx-rs-gui}
  install -Dm0644 -t "$pkgdir/usr/lib/systemd/system/" assets/snx-rs.service
}
