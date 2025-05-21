project('st-cmsis-c0', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.3.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-c0-1.3.0'

mcus = {
    'stm32c011d6': 'stm32c011__',
    'stm32c011f4': 'stm32c011__',
    'stm32c011f6': 'stm32c011__',
    'stm32c011j4': 'stm32c011__',
    'stm32c011j6': 'stm32c011__',
    'stm32c031c4': 'stm32c031__',
    'stm32c031c6': 'stm32c031__',
    'stm32c031f4': 'stm32c031__',
    'stm32c031f6': 'stm32c031__',
    'stm32c031g4': 'stm32c031__',
    'stm32c031g6': 'stm32c031__',
    'stm32c031k4': 'stm32c031__',
    'stm32c031k6': 'stm32c031__',
    'stm32c051c6': 'stm32c051__',
    'stm32c051c8': 'stm32c051__',
    'stm32c051d8': 'stm32c051__',
    'stm32c051f6': 'stm32c051__',
    'stm32c051f8': 'stm32c051__',
    'stm32c051g6': 'stm32c051__',
    'stm32c051g8': 'stm32c051__',
    'stm32c051k6': 'stm32c051__',
    'stm32c051k8': 'stm32c051__',
    'stm32c071c8': 'stm32c071__',
    'stm32c071cb': 'stm32c071__',
    'stm32c071f8': 'stm32c071__',
    'stm32c071fb': 'stm32c071__',
    'stm32c071g8': 'stm32c071__',
    'stm32c071gb': 'stm32c071__',
    'stm32c071k8': 'stm32c071__',
    'stm32c071kb': 'stm32c071__',
    'stm32c071r8': 'stm32c071__',
    'stm32c071rb': 'stm32c071__',
    'stm32c091cb': 'stm32c091__',
    'stm32c091cc': 'stm32c091__',
    'stm32c091fb': 'stm32c091__',
    'stm32c091fc': 'stm32c091__',
    'stm32c091gb': 'stm32c091__',
    'stm32c091gc': 'stm32c091__',
    'stm32c091kb': 'stm32c091__',
    'stm32c091kc': 'stm32c091__',
    'stm32c091rb': 'stm32c091__',
    'stm32c091rc': 'stm32c091__',
    'stm32c092cb': 'stm32c092__',
    'stm32c092cc': 'stm32c092__',
    'stm32c092ec': 'stm32c092__',
    'stm32c092fb': 'stm32c092__',
    'stm32c092fc': 'stm32c092__',
    'stm32c092gb': 'stm32c092__',
    'stm32c092gc': 'stm32c092__',
    'stm32c092kb': 'stm32c092__',
    'stm32c092kc': 'stm32c092__',
    'stm32c092rb': 'stm32c092__',
    'stm32c092rc': 'stm32c092__',
}
mcu = meson.get_external_property('mcu')
if mcu not in mcus
  mcus_keys = mcus.keys()
  error(f'Please specify external property \'mcu\' with one of this values: @mcus_keys@')
endif
type = mcus[mcu]

type_lower = type.replace('_', 'x')
system_src = files(root / 'Source' / 'Templates' / 'system_stm32c0xx.c')
startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
type_define = '-D' + type.to_upper().replace('_', 'x')
pkg_define = '-DSTM32C0'  # This can be used with universal stm32 header 

lib = static_library(
  f'st_cmsis_@type_lower@', system_src, startup_src,
  build_by_default: false,
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  c_args: type_define,
  cpp_args: type_define,
)

st_cmsis_c0_type_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  compile_args: [type_define, pkg_define],
  link_with: lib,
)
st_cmsis_c0_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: st_cmsis_c0_type_dep,
  link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
)