libzbar_sources = [
  'debug.h',
  'config.c',
  'error.c',
  'symbol.c',
  'image.c',
  'convert.c',
  'processor.c',
  'processor/lock.c',
  'refcnt.c',
  'timer.h',
  'mutex.h',
  'event.h',
  'thread.h',
  'window.c',
  'video.c',
  'img_scanner.c',
  'scanner.c',
  'decoder.c',
  'misc.c',
]

libzbar_deps = [
  dependency('libiconv'),
]

extra_libzbar_sources += [
  'svg.c',
]

if enable_ean
  libzbar_sources += [
    'decoder/ean.c',
  ]
endif
if enable_databar
  libzbar_sources += [
    'decoder/databar.c',
  ]
endif
if enable_code128
  libzbar_sources += [
    'decoder/code128.c',
  ]
endif
if enable_code93
  libzbar_sources += [
    'decoder/code93.c',
  ]
endif
if enable_code39
  libzbar_sources += [
    'decoder/code39.c',
  ]
endif
if enable_codabar
  libzbar_sources += [
    'decoder/codabar.c',
  ]
endif
if enable_i25
  libzbar_sources += [
    'decoder/i25.c',
  ]
endif
if enable_pdf417
  libzbar_sources += [
    'decoder/pdf417.c',
    'decoder/pdf417_hash.h',
  ]
endif
if enable_qrcode
  libzbar_sources += [
    'qrcode.h',
    'decoder/qr_finder.c',
    'qrcode/qrdec.c',
    'qrcode/qrdectxt.c',
    'qrcode/rs.c',
    'qrcode/isaac.c',
    'qrcode/bch15_5.c',
    'qrcode/binarize.c',
    'qrcode/util.c',
  ]
endif
if enable_sqcode
  libzbar_sources += [
    'sqcode.c',
    'decoder/sq_finder.c',
  ]
endif


if host_machine.system() == 'windows'

  # %-rc.o: %.rc
  #   $(RC) $(DEFS) $(DEFAULT_INCLUDES) $(INCLUDES) \
  #   $(AM_CPPFLAGS) $(CPPFLAGS) -o $@ $<
  # %-rc.lo: %.rc
  #   $(LIBTOOL) --tag=RC $(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) \
  #   --mode=compile $(RC) $(DEFS) $(DEFAULT_INCLUDES) $(INCLUDES) \
  #   $(AM_CPPFLAGS) $(CPPFLAGS) -o $@ $<

  libzbar_sources += [
    'processor/win.c',
  ]

  windows = import('windows')
  libzbar_sources += windows.compile_resources('libzbar.rc')

  libzbar_deps += dependency('threads')
else
  libzbar_sources += [
    'processor/posix.c',
  ]
endif

if have_v4l2
  libzbar_sources += [
    'video/v4l.c',
    'video/v4l2.c',
  ]
endif

if have_v4l1
  if not have_v4l2
    libzbar_sources += 'video/v4l.c'
  endif
  libzbar_sources += 'video/v4l1.c'
endif
if host_machine.system() == 'windows'
  if have_video
    if not with_directshow
      libzbar_sources += 'video/vfw.c'
      libzbar_deps += '-lvfw32'
    else
      libzbar_sources += 'video/dshow.c'
      libzbar_cflags += '-DCOBJMACROS'
      libzbar_deps += [
        '-loleaut32',
        '-lole32',
      ]
    endif
  endif
endif
if not have_video
  libzbar_sources += 'video/null.c'
endif

if have_libv4l
  libzbar_deps += dependency('v4l2')
endif

if have_jpeg
  libzbar_sources += [
    'jpeg.c'
  ]
endif

if have_x
  libzbar_sources += [
    'processor/x.c',
    'window/x.c',
    'window/ximage.c',
  ]
  libzbar_cppflags += $(X_CFLAGS)
  libzbar_ldflags += $(X_LIBS)
  libzbar_deps += $(X_PRE_LIBS) -lX11 $(X_EXTRA_LIBS)
  if have_xv
    libzbar_sources += 'window/xv.c'
    libzbar_deps += dependency('xv')
  endif
else
  if host_machine.system() == 'windows'
    libzbar_sources += [
      'window/win.c',
      'window/dib.c',
    ]
    # window/vfw.c -lvfw32
    libzbar_ldflags += [
      '-lgdi32',
      '-lwinmm',
    ]
  else
    libzbar_sources += [
      'processor/null.c',
      'window/null.c',
    ]
  endif
endif

if have_dbus
  libzbar_ldflags += $(DBUS_LIBS)
endif



library(
  'zbar',
  libzbar_sources,
  link_with: ,
  link_args: ,
  link_depends: ,
  include_directories: ,
  dependencies: libzbar_deps,
  install: true,
  build_by_default: false,
)


install_data(
  'zbar.pc',
  install_dir: pkgconfigdir,
)
