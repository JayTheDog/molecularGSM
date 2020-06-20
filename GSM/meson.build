gsm_sha1 = configure_file(
  input: 'GitSHA1.cpp.in',
  output: 'GitSHA1.cpp',
  configuration: configuration_data({
    'GIT_SHA1': run_command(find_program('git'), 'show', '-s', '--format=%h').stdout().strip(),
  })
)

srcs = files(
  'ase.cpp',
  'bmat.cpp',
  'eckart.cpp',
  'gaussian.cpp',
  'grad.cpp',
  'gstring.cpp',
  'icoord.cpp',
  'knnr.cpp',
  'mem.cpp',
  'mm_grad.cpp',
  'molpro.cpp',
  'mopac.cpp',
  'optic.cpp',
  'orca.cpp',
  'print.cpp',
  'pTable.cpp',
  'qchem.cpp',
  'qchemsf.cpp',
  'stringtools.cpp',
  'utils.cpp',
)

gsm_qchem = configure_file(
  input: 'qchem.h.in',
  output: 'qchem.h',
  format: 'cmake',
  configuration: configuration_data({
    'GSM_ENABLE_QCHEM': false,
    'GSM_ENABLE_QCHEM_SF': false,
    'GSM_ENABLE_MOLPRO': false,
    'GSM_ENABLE_ASE': false,
    'GSM_ENABLE_GAUSSIAN': false,
    'GSM_ENABLE_ORCA': true,
  })
)

incdir = include_directories('.')

gsm_lib = library(
  meson.project_name(),
  sources: [srcs, gsm_sha1],
  include_directories: incdir,
  dependencies: lib_deps,
)

gsm_dep = declare_dependency(
  link_with: gsm_lib,
  include_directories: incdir,
  dependencies: exe_deps,
)

gsm_exe = executable(
  '@0@.@1@'.format(meson.project_name(), 'orca'),
  sources: files('main.cpp'),
  dependencies: gsm_dep,
)
