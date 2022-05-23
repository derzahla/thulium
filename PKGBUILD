# Maintainer: derzahl <derzahl@ded.haus>
# Contributor: Evangelos Foutras <evangelos@foutrelis.com>
# Contributor: Pierre Schmitz <pierre@archlinux.de>
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>
# Contributor: Kevin Mihelich <kevin@archlinuxarm.org>

# Chromium, ungoogled, and compiled for Asahi linux

pkgname=thulium
pkgver=102.0.5005.61
pkgrel=1
_launcher_ver=8
_gcc_patchset=6
pkgdesc="A web browser built for speed, simplicity, and security"
arch=('aarch64')
url="https://www.chromium.org/Home"
license=('BSD')
options=(!mold !lto)
#FORCE_LIBCXX='yes'
depends=('gtk3' 'nss' 'alsa-lib' 'xdg-utils' 'libxss' 'libcups' 'libgcrypt'
         'ttf-liberation' 'systemd' 'dbus' 'libpulse' 'pciutils' 'libva'
         'desktop-file-utils' 'hicolor-icon-theme')
makedepends=('python' 'gn' 'ninja' 'clang' 'lld' 'gperf' 'nodejs' 'pipewire'
             'java-runtime-headless' 'git')
optdepends=('pipewire: WebRTC desktop sharing under Wayland'
            'kdialog: needed for file dialogs in KDE'
            'org.freedesktop.secrets: password storage backend on GNOME / Xfce'
            'kwallet: for storing passwords in KWallet on KDE desktops')

source=(https://commondatastorage.googleapis.com/chromium-browser-official/chromium-$pkgver.tar.xz
        chromium-launcher-$_launcher_ver.tar.gz::https://github.com/foutrelis/chromium-launcher/archive/v$_launcher_ver.tar.gz
        # Patchset
        https://github.com/stha09/chromium-patches/releases/download/chromium-${pkgver%%.*}-patchset-$_gcc_patchset/chromium-${pkgver%%.*}-patchset-$_gcc_patchset.tar.xz
        logos.zip
        0012-branding.patch
        0020-launcher-branding.patch
        0021-appdata-branding.patch
        sql-make-VirtualCursor-standard-layout-type.patch
        chromium-101-libxml-unbundle.patch
        chromium-102-disable-dawn.patch
        0001-widevine-support-for-arm.patch
        0002-Run-blink-bindings-generation-single-threaded.patch
        0003-Fix-eu-strip-build-for-newer-GCC.patch
        )

sha256sums=('1a3797d36901fa3ba63744b9a870b65a8890c9a850442c160196bc64df886b1f'
            '213e50f48b67feb4441078d50b0fd431df34323be15be97c55302d3fdac4483a'
            '23f2a772c4a6e31394d6ee7b8dbb5967d3b92bd859093444913377934bede594'
            '67baea4bf01a981d922cb1b6f937abbafabc78ae244725ca9d8c589afbee3e43'
            'c524cf6cfcfa00feff7f646f933aaa34d6ce7be04f64f8486f646fc9d4f80155'
            '1259cb3de4f7757d9308e4f1c71fedb77714a2721dd4629b918cd2eb65413ae3'
            '7e47ef4484ebb4d046fcd25c08bdb48ed5502fbc9499252fbaa869f81d0afa00'
            'b94b2e88f63cfb7087486508b8139599c89f96d7a4181c61fec4b4e250ca327a'
            'ea7a93442456a03549509022bca6f3a5e1600fa14caa062dd0fa0a6c45bbc9a8'
            'edb917ee0a244e3d85b57a52560c99c5aaa3fd00d6a6346910722f20a26045f9'
            'b05841e51807487f513f27cc85f8cbdf68306706afba2b03a5c5ad5965a29dc6'
            '56ceefb17b73eea37525380bf8105cd0fd8fcee0979cfcd3729a78471a35b6f2'
            '5ce5f3c9a4f76e704bb407a463587e9d71ed613478c17d3bc328e839c259bb0d'
            'eb70d0260f121faa6e2efd8e80a5e258f23474a214ff9f4f112bdcffdaaadd83'
            'c5cc6f52f22ef19490909d75cd0e7a1f48cdd35da0774327bf32634ede55413f'
            '8d83bf6cf8f91f5d934aa5be4c1684fbde94bf51e630a960d574d5e52cd69d89'
            '07bdc1b3fc8f0d0a4804d111c46ce3343cd7824de562f2848d429b917ce4bcfd'
            '34d08ea93cb4762cb33c7cffe931358008af32265fc720f2762f0179c3973574')
provides=('chromium')
conflicts=('chromium')
_uc_ver=101.0.4951.64-1
source=(${source[@]}
        $pkgname-$_uc_ver.tar.gz::https://github.com/Eloston/ungoogled-chromium/archive/$_uc_ver.tar.gz
        drirc-disable-10bpc-color-configs.conf
        ug-102.patch::https://github.com/Eloston/ungoogled-chromium/pull/1964.patch
        ozone-add-va-api-support-to-wayland.patch
        wayland-egl.patch)
# Possible replacements are listed in build/linux/unbundle/replace_gn_files.py
# Keys are the names in the above script; values are the dependencies in Arch
declare -gA _system_libs=(
  #[ffmpeg]=ffmpeg
  [flac]=flac
  [fontconfig]=fontconfig
  [freetype]=freetype2
  [harfbuzz-ng]=harfbuzz
  [icu]=icu
  [libdrm]=
  [libjpeg]=libjpeg
  [libpng]=libpng
  #[libvpx]=libvpx
  [libwebp]=libwebp
  [libxml]=libxml2
  [re2]=re2
  [snappy]=snappy
  [libxslt]=libxslt
  [opus]=opus
  [zlib]=minizip
)

# Unbundle only without libc++, as libc++ is not fully ABI compatible with libstdc++
if [[ ${FORCE_LIBCXX} != yes ]]; then
  _system_libs+=(
    [re2]=re2
    [snappy]=snappy
  )
fi

_unwanted_bundled_libs=(
  $(printf "%s\n" ${!_system_libs[@]} | sed 's/^libjpeg$/&_turbo/')
)
depends+=(${_system_libs[@]})

# Taken from chromium-dev PKGBUILD
if [ ! -f "${BUILDDIR}/PKGBUILD" ]; then
  _builddir="/${pkgname}"
fi

_clang_path="${BUILDDIR}${_builddir}/src/chromium-${pkgver}/third_party/llvm-build/Release+Asserts/bin/"

prepare() {
  cd "$srcdir/chromium-$pkgver"

  # Allow building against system libraries in official builds
  sed -i 's/OFFICIAL_BUILD/GOOGLE_CHROME_BUILD/' \
    tools/generate_shim_headers/generate_shim_headers.py

  # Arch Linux ARM fixes
  patch -p1 -i ../0001-widevine-support-for-arm.patch
  patch -p1 -i ../0002-Run-blink-bindings-generation-single-threaded.patch
  patch -p1 -i ../0003-Fix-eu-strip-build-for-newer-GCC.patch

  if [[ $CARCH == "armv7h" ]]; then
    export ALARM_NINJA_JOBS="4"
    export MAKEFLAGS="-j4"
  fi

  # Allow build to set march and options on AArch64 (crc, crypto)
  [[ $CARCH == "aarch64" ]] && CFLAGS=`echo $CFLAGS | sed -e 's/-march=armv8-a//'` && CXXFLAGS="$CFLAGS"

  # https://crbug.com/893950
  sed -i -e 's/\<xmlMalloc\>/malloc/' -e 's/\<xmlFree\>/free/' \
    third_party/blink/renderer/core/xml/*.cc \
    third_party/blink/renderer/core/xml/parser/xml_document_parser.cc \
    third_party/libxml/chromium/*.cc \
    third_party/maldoca/src/maldoca/ole/oss_utils.h

  # Apply patches if Google Clang is not used.
  if [[ ${GOOGLE_CLANG} != yes ]]; then
    patch -Np1 -i ../sql-make-VirtualCursor-standard-layout-type.patch
  fi

  # Apply patches if libc++ is not used.
  #if [[ ${FORCE_LIBCXX} != yes ]]; then
  #  patch -Np1 -i ../chromium-102-autofill-IWYU.patch
  #fi

  # Custom or upstream patches.
  patch -Np1 -i ../chromium-101-libxml-unbundle.patch
  patch -Np0 -i ../chromium-102-disable-dawn.patch
  #patch -Np0 -i ../chromium-102-no-opaque-pointers.patch
  patch -Np1 -i ../0021-appdata-branding.patch


  # Alternative to removing the orchestrator.
  touch third_party/blink/tools/merge_web_test_results.pydeps
  mkdir -p third_party/blink/tools/blinkpy/web_tests
  touch third_party/blink/tools/blinkpy/web_tests/merge_results.pydeps

  mkdir -p third_party/node/linux/node-linux-x64/bin
  mkdir -p third_party/node/linux/node-linux-arm64/bin
  mkdir -p third_party/node/mac/node-darwin-arm64/bin
  ln -sf /usr/bin/node third_party/node/linux/node-linux-x64/bin/
  ln -sf /usr/bin/node third_party/node/linux/node-linux-arm64/bin/
  ln -sf /usr/bin/node third_party/node/mac/node-darwin-arm64/bin/
  ln -s /usr/bin/java third_party/jdk/current/bin/
  _ug_repo="$srcdir/ungoogled-chromium-$_uc_ver"
  cd $_ug_repo ; patch -Np1 -i ../ug-102.patch ; cd "$srcdir/chromium-$pkgver"
  _utils="${_ug_repo}/utils"   
  python "$_utils/prune_binaries.py" ./ "$_ug_repo/pruning.list"
  python "$_utils/patches.py" apply ./ "$_ug_repo/patches"
  python "$_utils/domain_substitution.py" apply -r "$_ug_repo/domain_regex.list" \
    -f "$_ug_repo/domain_substitution.list" -c domainsubcache.tar.gz ./

  # Remove bundled libraries for which we will use the system copies; this
  # Ungoogled Chromium changes
  # *should* do what the remove_bundled_libraries.py script does, with the
  # added benefit of not having to list all the remaining libraries
  local _lib
  for _lib in ${_unwanted_bundled_libs[@]}; do
    find "third_party/$_lib" -type f \
      \! -path "third_party/$_lib/chromium/*" \
      \! -path "third_party/$_lib/google/*" \
      \! -path "third_party/harfbuzz-ng/utils/hb_scoped.h" \
      \! -regex '.*\.\(gn\|gni\|isolate\)' \
      -delete
  done

  python build/linux/unbundle/replace_gn_files.py \
    --system-libraries "${!_system_libs[@]}"

  # Download Google's prebuilt Clang if needed.
  if [[ ${GOOGLE_CLANG} == yes ]]; then
    tools/clang/scripts/update.py
  fi


  # If using bundled ffmpeg, create link to system opus headers. Compiling fails without this.
  if [[ -z ${_system_libs[ffmpeg]+set} ]]; then
    rm -fr third_party/opus/src/include
    ln -sf /usr/include/opus/ third_party/opus/src/include
  fi
  
  # apply all in ./patches
  for p in $(ls -1 ../patches); do
    patch -Np1 -i ../patches/$p
  done

  cd chrome
  patch -Np1 -i ../../0012-branding.patch

  # Applying Chromium launcher patches
  cd "$srcdir/chromium-launcher-$_launcher_ver"
  patch -Np1 -i ../0020-launcher-branding.patch

}

build() {
  unset CFLAGS
  make -C chromium-launcher-$_launcher_ver

  # Doing this should lower the amount of misses for ccache.
  if check_buildoption ccache y; then
    export CCACHE_BASEDIR="$(pwd)"
  fi

  cd "$srcdir/chromium-$pkgver"

# Rebuild eu-strip
#  pushd buildtools/third_party/eu-strip
#  ./build.sh
#  popd


  if check_buildoption ccache y; then
    # Avoid falling back to preprocessor mode when sources contain time macros
    export CCACHE_SLOPPINESS=time_macros
  fi

  # Switch between Google's Clang and system Clang.
  if [[ ${GOOGLE_CLANG} == yes ]]; then
    export CC="${_clang_path}clang"
    export CXX="${_clang_path}clang++"
    export AR="${_clang_path}llvm-ar"
  else
    export CC=clang
    export CXX=clang++
    export AR=ar
  fi
  export NM=nm
  export CFLAGS="-march=armv8.5-a -mcpu=apple-m1 -mtune=apple-m1 -O3 -pipe -fstack-protector-strong -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=2 -Wformat -Werror=format-security"
  export CXXFLAGS=$CFLAGS

  local _flags=(
    'custom_toolchain="//build/toolchain/linux/unbundle:default"'
    'host_toolchain="//build/toolchain/linux/unbundle:default"'
    'clang_use_chrome_plugins=false'
    'clang_use_default_sample_profile=false'
    'use_allocator="none"'
    'is_official_build=true' # implies is_cfi=true on x86_64
    'symbol_level=0' # sufficient for backtraces on x86(_64)
    'is_cfi=false'
    'treat_warnings_as_errors=false'
    'disable_fieldtrial_testing_config=true'
    'blink_enable_generated_code_formatting=false'
    'ffmpeg_branding="Chrome"'
    'proprietary_codecs=true'
    'rtc_use_pipewire=true'
    'link_pulseaudio=true'
    'use_gnome_keyring=false'
    'use_gold=false'
    'use_lld=true'
    'use_sysroot=false'
    'enable_hangout_services_extension=true'
    'enable_widevine=true'
    'enable_nacl=false'
    'use_vaapi=true'
    'use_ozone=true'
    'ozone_auto_platforms=false'
    'ozone_platform_headless=true'
    'ozone_platform_x11=true'
    'ozone_platform="x11"'
  )

  # PGO profiles cannot be read with system Clang.
  if [[ ${GOOGLE_CLANG} != yes ]]; then
    _flags+=('chrome_pgo_phase=0')
  fi

  if [[ -n ${_system_libs[icu]+set} ]]; then
    _flags+=('icu_use_data_file=false')
  fi

  if [[ ${FORCE_LIBCXX} == yes ]]; then
    _flags+=('use_custom_libcxx=true')
  else
    _flags+=('use_custom_libcxx=false')
  fi

  # Append ungoogled chromium flags to _flags array
  _ug_repo="$srcdir/ungoogled-chromium-$_uc_ver"
  readarray -t -O ${#_flags[@]} _flags < "${_ug_repo}/flags.gn"


  # Taken from chromium-dev
#  if [[ -z ${_system_libs[ffmpeg]+set} ]]; then
#    msg2 "Build bundled ffmpeg, compilation fails with system ffmpeg"
#    pushd third_party/ffmpeg &> /dev/null
#    chromium/scripts/build_ffmpeg.py linux x64 --branding Chrome -- \
#      --disable-asm
#
#    chromium/scripts/copy_config.sh
#    chromium/scripts/generate_gn.py
#    popd &> /dev/null
#  fi

  # Facilitate deterministic builds (taken from build/config/compiler/BUILD.gn)
  CFLAGS+='   -Wno-builtin-macro-redefined'
  CXXFLAGS+=' -Wno-builtin-macro-redefined'
  CPPFLAGS+=' -D__DATE__=  -D__TIME__=  -D__TIMESTAMP__='

  # Do not warn about unknown warning options
  CFLAGS+='   -Wno-unknown-warning-option'
  CXXFLAGS+=' -Wno-unknown-warning-option'

  # Let Chromium set its own symbol level
  CFLAGS=${CFLAGS/-g }
  CXXFLAGS=${CXXFLAGS/-g }

  # Get rid of the "-fexceptions" flag.
  CFLAGS=${CFLAGS/-fexceptions}
  CFLAGS=${CFLAGS/-fcf-protection}
  CXXFLAGS=${CXXFLAGS/-fexceptions}
  CXXFLAGS=${CXXFLAGS/-fcf-protection}

  # This appears to cause random segfaults when combined with ThinLTO
  # https://bugs.archlinux.org/task/73518
  CFLAGS=${CFLAGS/-fstack-clash-protection}
  CXXFLAGS=${CXXFLAGS/-fstack-clash-protection}

  # https://crbug.com/957519#c122
  CXXFLAGS=${CXXFLAGS/-Wp,-D_GLIBCXX_ASSERTIONS}

  # Specific to libstdc++
  CXXFLAGS+=' -fbracket-depth=512'

  gn gen out/Release --args="${_flags[*]}"
  ninja -C out/Release chrome chrome_sandbox chromedriver
}

package() {
  cd chromium-launcher-$_launcher_ver
  make PREFIX=/usr DESTDIR="$pkgdir" install
  install -Dm644 LICENSE \
    "$pkgdir/usr/share/licenses/thulium/LICENSE.launcher"

  cd "$srcdir/chromium-$pkgver"

  # Install binaries
  install -D out/Release/chrome "$pkgdir/usr/lib/$pkgname/$pkgname"
  install -D out/Release/chromedriver "$pkgdir/usr/lib/$pkgname/thuledriver"
  install -Dm4755 out/Release/chrome_sandbox "$pkgdir/usr/lib/$pkgname/chrome-sandbox"
  ln -s /usr/lib/$pkgname/thuledriver "$pkgdir/usr/bin/thuledriver"

  # needed for vaapi
  install -Dm644 ../drirc-disable-10bpc-color-configs.conf \
    "$pkgdir/usr/share/drirc.d/10-$pkgname.conf"


  # Install .desktop and manpages.
  install -Dm644 chrome/installer/linux/common/desktop.template \
    "$pkgdir/usr/share/applications/thulium.desktop"
  install -Dm644 chrome/app/resources/manpage.1.in \
    "$pkgdir/usr/share/man/man1/thulium.1"
  sed -i \
    -e "s/@@MENUNAME@@/Thulium/g" \
    -e "s/@@PACKAGE@@/thulium/g" \
    -e "s/@@USR_BIN_SYMLINK_NAME@@/thulium/g" \
    "$pkgdir/usr/share/applications/thulium.desktop" \
    "$pkgdir/usr/share/man/man1/thulium.1"

  install -Dm644 chrome/installer/linux/common/chromium-browser/chromium-browser.appdata.xml \
    "$pkgdir/usr/share/metainfo/thulium.appdata.xml"
  sed -ni \
    -e 's/chromium-browser\.desktop/thulium.desktop/' \
    -e '/<update_contact>/d' \
    -e '/<p>/N;/<p>\n.*\(We invite\|Chromium supports Vorbis\)/,/<\/p>/d' \
    -e '/^<?xml/,$p' \
    "$pkgdir/usr/share/metainfo/thulium.appdata.xml"

  # Install resources and locales.
  cp \
    out/Release/{chrome_{100,200}_percent,resources}.pak \
    out/Release/{v8_context_snapshot.bin,chrome_crashpad_handler} \
    out/Release/lib{EGL,GLESv2}.so \
    out/Release/{libvk_swiftshader.so,vk_swiftshader_icd.json} \
    "$pkgdir/usr/lib/thulium/"

  if [[ -z ${_system_libs[icu]+set} ]]; then
    cp out/Release/icudtl.dat "$pkgdir/usr/lib/thulium/"
  fi

  install -Dm644 -t "$pkgdir/usr/lib/thulium/locales" out/Release/locales/*.pak
#  install -Dm755 -t "$pkgdir/usr/lib/thulium/swiftshader" out/Release/swiftshader/*.so

  # Install icons.
  for size in 16 22 24 32 48 64 128 256; do
    install -Dm644 "../product_logo_$size.png" \
      "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/$pkgname.png"
  done


  for size in 16 32; do
    install -Dm644 "chrome/app/theme/default_100_percent/chromium/product_logo_$size.png" \
      "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/thulium.png"
  done

  # Install license.
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/thulium/LICENSE"
}

# vim:set ts=2 sw=2 et:
