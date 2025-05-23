project('st-cmsis-f7', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.2.9',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-f7-1.2.9'

mcus = {
    'stm32f722ic': 'stm32f722__',
    'stm32f722ie': 'stm32f722__',
    'stm32f722rc': 'stm32f722__',
    'stm32f722re': 'stm32f722__',
    'stm32f722vc': 'stm32f722__',
    'stm32f722ve': 'stm32f722__',
    'stm32f722zc': 'stm32f722__',
    'stm32f722ze': 'stm32f722__',
    'stm32f723ic': 'stm32f723__',
    'stm32f723ie': 'stm32f723__',
    'stm32f723vc': 'stm32f723__',
    'stm32f723ve': 'stm32f723__',
    'stm32f723zc': 'stm32f723__',
    'stm32f723ze': 'stm32f723__',
    'stm32f730i8': 'stm32f730__',
    'stm32f730r8': 'stm32f730__',
    'stm32f730v8': 'stm32f730__',
    'stm32f730z8': 'stm32f730__',
    'stm32f732ie': 'stm32f732__',
    'stm32f732re': 'stm32f732__',
    'stm32f732ve': 'stm32f732__',
    'stm32f732ze': 'stm32f732__',
    'stm32f733ie': 'stm32f733__',
    'stm32f733ve': 'stm32f733__',
    'stm32f733ze': 'stm32f733__',
    'stm32f745ie': 'stm32f745__',
    'stm32f745ig': 'stm32f745__',
    'stm32f745ve': 'stm32f745__',
    'stm32f745vg': 'stm32f745__',
    'stm32f745ze': 'stm32f745__',
    'stm32f745zg': 'stm32f745__',
    'stm32f746be': 'stm32f746__',
    'stm32f746bg': 'stm32f746__',
    'stm32f746ie': 'stm32f746__',
    'stm32f746ig': 'stm32f746__',
    'stm32f746ne': 'stm32f746__',
    'stm32f746ng': 'stm32f746__',
    'stm32f746ve': 'stm32f746__',
    'stm32f746vg': 'stm32f746__',
    'stm32f746ze': 'stm32f746__',
    'stm32f746zg': 'stm32f746__',
    'stm32f750n8': 'stm32f750__',
    'stm32f750v8': 'stm32f750__',
    'stm32f750z8': 'stm32f750__',
    'stm32f756bg': 'stm32f756__',
    'stm32f756ig': 'stm32f756__',
    'stm32f756ng': 'stm32f756__',
    'stm32f756vg': 'stm32f756__',
    'stm32f756zg': 'stm32f756__',
    'stm32f765bg': 'stm32f765__',
    'stm32f765bi': 'stm32f765__',
    'stm32f765ig': 'stm32f765__',
    'stm32f765ii': 'stm32f765__',
    'stm32f765ng': 'stm32f765__',
    'stm32f765ni': 'stm32f765__',
    'stm32f765vg': 'stm32f765__',
    'stm32f765vi': 'stm32f765__',
    'stm32f765zg': 'stm32f765__',
    'stm32f765zi': 'stm32f765__',
    'stm32f767bg': 'stm32f767__',
    'stm32f767bi': 'stm32f767__',
    'stm32f767ig': 'stm32f767__',
    'stm32f767ii': 'stm32f767__',
    'stm32f767ng': 'stm32f767__',
    'stm32f767ni': 'stm32f767__',
    'stm32f767vg': 'stm32f767__',
    'stm32f767vi': 'stm32f767__',
    'stm32f767zg': 'stm32f767__',
    'stm32f767zi': 'stm32f767__',
    'stm32f769ai': 'stm32f769__',
    'stm32f769bg': 'stm32f769__',
    'stm32f769bi': 'stm32f769__',
    'stm32f769ig': 'stm32f769__',
    'stm32f769ii': 'stm32f769__',
    'stm32f769ng': 'stm32f769__',
    'stm32f769ni': 'stm32f769__',
    'stm32f777bi': 'stm32f777__',
    'stm32f777ii': 'stm32f777__',
    'stm32f777ni': 'stm32f777__',
    'stm32f777vi': 'stm32f777__',
    'stm32f777zi': 'stm32f777__',
    'stm32f778ai': 'stm32f779__',
    'stm32f779ai': 'stm32f779__',
    'stm32f779bi': 'stm32f779__',
    'stm32f779ii': 'stm32f779__',
    'stm32f779ni': 'stm32f779__',
}
mcu = meson.get_external_property('mcu')
if mcu not in mcus
  mcus_keys = mcus.keys()
  error(f'Please specify external property \'mcu\' with one of this values: @mcus_keys@')
endif
type = mcus[mcu]

type_lower = type.replace('_', 'x')
system_src = files(root / 'Source' / 'Templates' / 'system_stm32f7xx.c')
startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
type_define = '-D' + type.to_upper().replace('_', 'x')
pkg_define = '-DSTM32F7'  # This can be used with universal stm32 header 

lib = static_library(
  f'st_cmsis_@type_lower@', system_src, startup_src,
  build_by_default: false,
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  c_args: type_define,
  cpp_args: type_define,
)

st_cmsis_f7_type_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  compile_args: [type_define, pkg_define],
  link_with: lib,
)
st_cmsis_f7_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: st_cmsis_f7_type_dep,
  link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
)