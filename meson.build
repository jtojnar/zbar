project(
  'zbar',
  [
    'c',
    'c++',
  ],
  version: '0.23',
)

# update these just before each release (along w/package version above)
#   LIB_VERSION update instructions copied from libtool docs:
#   library version follows the form current:revision:age
#   - If the library source code has changed at all since the last update,
#     then increment revision (c:r:a becomes c:r+1:a).
#   - If any interfaces have been added, removed, or changed,
#     increment current, and set revision to 0.
#   - If any interfaces have been added since the last public release,
#     then increment age.
#   - If any interfaces have been removed since the last public release,
#     then set age to 0.
LIB_VERSION = '3:0:3'
RELDATE = '2017-04-11'

# widget libraries use their own versioning.
# NB pygtk wrapper is *unversioned*
ZGTK_LIB_VERSION = '0:2:0'
ZQT_LIB_VERSION = '1:2:1'

zbar_version_components = (meson.project_version() + '.0').split('.') # add a fallback patch number
ZBAR_VERSION_MAJOR = zbar_version_components[0]
ZBAR_VERSION_MINOR = zbar_version_components[1]
ZBAR_VERSION_PATCH = zbar_version_components[2]

lib_version_components = (meson.project_version() + '.0').split('.') # add a fallback patch number
cur = lib_version_components[0]
age = lib_version_components[2]
LIB_VERSION_MAJOR = (cur.to_int() - age.to_int()).to_string()
LIB_VERSION_MINOR = age
LIB_VERSION_REVISION = lib_version_components[1]

host_os = host_machine.system().to_lower()

platform_linux = (
  host_os.contains('linux')
)

platform_win32 = (
  host_os.contains('mingw') or
  host_os.contains('cygwin') or
  host_os.contains('windows')
)

platform_osx = (
  host_os.contains('machten') or
  host_os.contains('rhapsody') or
  host_os.contains('darwin')
)

symbologies = {
  'ean': 'EAN symbologies',
  'databar': 'DataBar symbology',
  'code128': 'Code 128 symbology',
  'code93': 'Code 93 symbology',
  'code39': 'Code 39 symbology',
  'codabar': 'Codabar symbology',
  'i25': 'Interleaved 2 of 5 symbology',
  'qrcode': 'QR Code',
  'sqcode': 'SQ Code',
  'pdf417': 'PDF417 symbology (incomplete)',
}

libdir = get_option('libdir')
pkgconfigdir = libdir / 'pkgconfig'

# symbologies
codes = get_option('codes')
enable_ean = codes.contains('ean')
enable_databar = codes.contains('databar')
enable_code128 = codes.contains('code128')
enable_code93 = codes.contains('code93')
enable_code39 = codes.contains('code39')
enable_codabar = codes.contains('codabar')
enable_i25 = codes.contains('i25')
enable_pdf417 = codes.contains('pdf417')
enable_qrcode = codes.contains('qrcode')
enable_sqcode = codes.contains('sqcode')

threads_dep = dependency(
  'threads',
  required: get_option('threads'),
)

dbus_dep = dependency(
  'dbus-1',
  version: '>= 1.0',
  not_found_message: 'DBus development libraries not found',
  required: get_option('dbus')
)

enable_doc = get_option('doc')
enable_video = get_option('video')
with_directshow = get_option('directshow')

if enable_video
  if platform_win32
    if with_directshow
      with_video = 'directshow'
    else
      have_vfw = cc.has_header('vfw.h')

      if not have_vfw
        error('''
          test for VfW video support failed!
          configure -Dvideo=false to skip building video support.
        ''')
      endif
    endif
  else
    v4l2_dep = dependency(
      'libv4l2',
      required: false,
    )

    have_v4l1 = cc.has_header('linux/videodev.h')
    have_v4l2 = cc.has_header('linux/videodev2.h')
    have_libv4l = v4l2_dep.found()

    if not have_v4l1 and not have_v4l2
      error('''
        test for video support failed!
        rebuild your kernel to include video4linux support or
        configure -Dvideo=false to skip building video support.]
      ''')
    endif

    if not have_v4l2
      warning('v4l2 API not detected, upgrade your kernel!')
    endif
  endif

  have_video = true
else
  have_video = false
endif

have_jpeg
have_x
have_xv
have_dbus = dbus.found()

subdir('zbar')
