# Maintainer: Mahmut Dikcizgi <boogiepop a~t gmx com>
# Contributor: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>
# Contributor: Paul Mattal <paul@archlinux.org>

# ALARM: Kevin Mihelich <kevin@archlinuxarm.org>
#  - use -fPIC in host cflags for v7/v8 to fix print_options.c compile
#  - remove makedepends on ffnvcodec-headers, remove --enable-nvenc, --enable-nvdec
#  - remove depends on aom, remove --enable-libaom
#  - remove depends on intel-media-sdk, remove --enable-libmfx
#  - remove depends on vmaf, remove --enable-vmaf
#  - remove depends on rav1e, remove --enable-librav1e
#  - remove depends on svt-av1, remove --enable-libsvtav1
#  - remove --enable-lto
#  - enable rockchip decoders witht he highest priority
#  - interface rockchip rga from kernel to userspace directly
#  - hack around rockchips vp8&9 colorspace is not detected when used with Firefox

pkgname=ffmpeg-mpp-git
pkgver=6.1.r112619.99ea69d6d4
pkgrel=1
_obs_deps_tag=2023-04-03
_github_user=nyanmisaka
_github_repo=ffmpeg-rockchip
_github_branch=master
pkgdesc='Complete solution to record, convert and stream audio and video supporting rockchip MPP hardware decoder'
arch=(aarch64 arm7f)
url="https://github.com/$_github_user/$_github_repo.git"
license=(GPL3)
options=(!lto strip)
depends=(
  aom
  alsa-lib
  cairo
  bzip2
  fontconfig
  fribidi
  gmp
  gnutls
  gsm
  jack
  lame
  libass
  libavc1394
  libbluray
  libbs2b
  dav1d
  libdrm
  freetype2
  libgl
  libiec61883
  libjxl
  libmodplug
  libopenmpt
  libpulse
  libraw1394
  librsvg
  libsoxr
  libssh
  libtheora
  libva
  libvdpau
  vid.stab
  libvorbis
  libvpx	
  libwebp
  libx11
  x264
  x265
  libxcb
  libxext
  libxml2
  libxv
  xvidcore
  zimg
  ocl-icd
  onevpl
  opencore-amr
  openjpeg2
  opus
  rav1e
  sdl2
  speex
  srt
  v4l-utils
  xz
  zlib
  mpp
  librist
  librga-multi
)
makedepends=(
  amf-headers
  avisynthplus
  clang
  git
  ladspa
  mesa
  nasm
  opencl-headers
  mpp
  libyuv
  perl
)

optdepends=(
  'avisynthplus: AviSynthPlus support'
  'ladspa: LADSPA filters'
)

provides=(
  libavcodec.so
  libavdevice.so
  libavfilter.so
  libavformat.so
  libavutil.so
  libpostproc.so
  libswresample.so
  libswscale.so
  "ffmpeg=${pkgver}-${pkgrel}"
  "ffmpeg-obs=${pkgver}-${pkgrel}"
  "ffmpeg-rockchip"
  "ffmpeg-rockchip-git"
)

conflicts=(
  ffmpeg
  $pkgname
)

# to prevent multiple previous ffmpeg packages colliding
replaces=(
  ffmpeg-rockchip
  ffmpeg-rockchip-git
)

_url_ffmpeg="https://github.com/$_github_user/$_github_repo#branch=$_github_branch"

source=(
  "$_url_ffmpeg"
  "obs-deps::git+https://github.com/obsproject/obs-deps.git#tag=${_obs_deps_tag}"
  add-av_stream_get_first_dts-for-chromium.patch
)

b2sums=('SKIP'
        'SKIP'
        '555274228e09a233d92beb365d413ff5c718a782008075552cafb2130a3783cf976b51dfe4513c15777fb6e8397a34122d475080f2c4483e8feea5c0d878e6de')

validpgpkeys=(DD1EC9E8DE085C629B3E1846B18E8928B3948D64) # Michael Niedermayer <michael@niedermayer.cc>

prepare() {
  cd ffmpeg-rockchip
  sed -i 's/RTLD_LOCAL/RTLD_DEEPBIND/g' libavformat/avisynth.c
  patch -Np1 -i ../add-av_stream_get_first_dts-for-chromium.patch # https://crbug.com/1251779
  
  # This patch applies:
  #  - Fix decoding of certain malformed FLV files
  #  - Add additional CPU levels for libaom
  patch -Np1 -i ../obs-deps/deps.ffmpeg/patches/FFmpeg/0001-flvdec-handle-unknown.patch
  patch -Np1 -i ../obs-deps/deps.ffmpeg/patches/FFmpeg/0002-libaomenc-presets.patch
}

pkgver() {
  cd ffmpeg-rockchip
  printf "%s.r%s.%s" "$(cat RELEASE)" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd ffmpeg-rockchip
  [[ $CARCH == "armv7h" || $CARCH == "aarch64" ]] && CONFIG='--host-cflags="-fPIC"'
  ./configure \
    --prefix=/usr \
    --disable-debug \
    --disable-static \
    --disable-stripping \
    --enable-amf \
    --enable-avisynth \
    --enable-cuda-llvm \
    --enable-fontconfig \
    --enable-gmp \
    --enable-gnutls \
    --enable-gpl \
    --enable-ladspa \
    --enable-libaom \
    --enable-libass \
    --enable-libbluray \
    --enable-libbs2b \
    --enable-libdav1d \
    --enable-libdrm \
    --enable-libfreetype \
    --enable-libfribidi \
    --enable-libgsm \
    --enable-libiec61883 \
    --enable-libjack \
    --enable-libjxl \
    --enable-libmodplug \
    --enable-libmp3lame \
    --enable-libopencore_amrnb \
    --enable-libopencore_amrwb \
    --enable-libopenjpeg \
    --enable-libopenmpt \
    --enable-libopus \
    --enable-libpulse \
    --enable-librav1e \
    --enable-librsvg \
    --enable-libsoxr \
    --enable-libspeex \
    --enable-libsrt \
    --enable-libssh \
    --enable-libtheora \
    --enable-libv4l2 \
    --enable-libvidstab \
    --enable-libvorbis \
    --enable-libvpl \
    --enable-libvpx \
    --enable-libwebp \
    --enable-libx264 \
    --enable-libx265 \
    --enable-libxcb \
    --enable-libxml2 \
    --enable-libxvid \
    --enable-libzimg \
    --enable-opencl \
    --enable-opengl \
    --enable-shared \
    --enable-version3 \
    --enable-librist \
    --disable-vulkan \
    --disable-doc \
    --enable-rkmpp \
    --enable-rkrga  $CONFIG
  make ${MAKEFLAGS}
  make ${MAKEFLAGS} tools/qt-faststart
  make ${MAKEFLAGS} doc/ff{mpeg,play}.1
}

package() {
  make ${MAKEFLAGS} DESTDIR="${pkgdir}" -C ffmpeg-rockchip install install-man
  install -Dm 755 ffmpeg-rockchip/tools/qt-faststart "${pkgdir}"/usr/bin/
}

# vim: ts=2 sw=2 et:
