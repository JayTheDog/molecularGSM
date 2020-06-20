cxx = meson.get_compiler('cpp')
lib_deps = []
exe_deps = []

static = get_option('default_library') == 'static'
la_backend = get_option('la_backend')
openmp = get_option('openmp')
if la_backend == 'mkl'

  if cxx.get_id() == 'gcc'
    mkl_dep = [cxx.find_library('mkl_gf_lp64')]
    mkl_dep += cxx.find_library('mkl_gnu_thread')
  elif cxx.get_id() == 'intel'
    mkl_dep = [cxx.find_library('mkl_intel_lp64')]
    mkl_dep += cxx.find_library('mkl_intel_thread')
  endif
  mkl_dep += cxx.find_library('mkl_core')

  lib_deps += mkl_dep

elif la_backend == 'mkl-rt'
  mkl_dep = fc.find_library('mkl_rt', required: true)
  if fc.get_id() == 'intel'
    exe_deps += fc.find_library('ifcore')
  endif

  lib_deps += mkl_dep

endif


if openmp
  omp_dep = dependency(
    'openmp',
    required: cxx.get_id() != 'intel',
    static: static,
    language: 'cpp'
  )
  if not omp_dep.found()
    omp_dep = declare_dependency(
      compile_args: '-qopenmp',
      link_args: '-qopenmp',
    )
  endif
  lib_deps += omp_dep
endif

exe_deps += lib_deps
if get_option('default_library') == 'static'
  exe_deps += declare_dependency(link_args: '-static')
endif
