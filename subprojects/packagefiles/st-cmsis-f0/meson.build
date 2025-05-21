project('st-cmsis-f0', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '2.3.7',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-f0-2.3.7'

mcus = {
    'stm32f030c6': 'stm32f030_6',
    'stm32f030c8': 'stm32f030_8',
    'stm32f030cc': 'stm32f030_c',
    'stm32f030f4': 'stm32f030_6',
    'stm32f030k6': 'stm32f030_6',
    'stm32f030r8': 'stm32f030_8',
    'stm32f030rc': 'stm32f030_c',
    'stm32f031c4': 'stm32f031_6',
    'stm32f031c6': 'stm32f031_6',
    'stm32f031e6': 'stm32f031_6',
    'stm32f031f4': 'stm32f031_6',
    'stm32f031f6': 'stm32f031_6',
    'stm32f031g4': 'stm32f031_6',
    'stm32f031g6': 'stm32f031_6',
    'stm32f031k4': 'stm32f031_6',
    'stm32f031k6': 'stm32f031_6',
    'stm32f038c6': 'stm32f038__',
    'stm32f038e6': 'stm32f038__',
    'stm32f038f6': 'stm32f038__',
    'stm32f038g6': 'stm32f038__',
    'stm32f038k6': 'stm32f038__',
    'stm32f042c4': 'stm32f042_6',
    'stm32f042c6': 'stm32f042_6',
    'stm32f042f4': 'stm32f042_6',
    'stm32f042f6': 'stm32f042_6',
    'stm32f042g4': 'stm32f042_6',
    'stm32f042g6': 'stm32f042_6',
    'stm32f042k4': 'stm32f042_6',
    'stm32f042k6': 'stm32f042_6',
    'stm32f042t6': 'stm32f042_6',
    'stm32f048c6': 'stm32f048__',
    'stm32f048g6': 'stm32f048__',
    'stm32f048t6': 'stm32f048__',
    'stm32f051c4': 'stm32f051_8',
    'stm32f051c6': 'stm32f051_8',
    'stm32f051c8': 'stm32f051_8',
    'stm32f051k4': 'stm32f051_8',
    'stm32f051k6': 'stm32f051_8',
    'stm32f051k8': 'stm32f051_8',
    'stm32f051r4': 'stm32f051_8',
    'stm32f051r6': 'stm32f051_8',
    'stm32f051r8': 'stm32f051_8',
    'stm32f051t8': 'stm32f051_8',
    'stm32f058c8': 'stm32f058__',
    'stm32f058r8': 'stm32f058__',
    'stm32f058t8': 'stm32f058__',
    'stm32f070c6': 'stm32f070_6',
    'stm32f070cb': 'stm32f070_b',
    'stm32f070f6': 'stm32f070_6',
    'stm32f070rb': 'stm32f070_b',
    'stm32f071c8': 'stm32f071_b',
    'stm32f071cb': 'stm32f071_b',
    'stm32f071rb': 'stm32f071_b',
    'stm32f071v8': 'stm32f071_b',
    'stm32f071vb': 'stm32f071_b',
    'stm32f072c8': 'stm32f072_b',
    'stm32f072cb': 'stm32f072_b',
    'stm32f072r8': 'stm32f072_b',
    'stm32f072rb': 'stm32f072_b',
    'stm32f072v8': 'stm32f072_b',
    'stm32f072vb': 'stm32f072_b',
    'stm32f078cb': 'stm32f078__',
    'stm32f078rb': 'stm32f078__',
    'stm32f078vb': 'stm32f078__',
    'stm32f091cb': 'stm32f091_c',
    'stm32f091cc': 'stm32f091_c',
    'stm32f091rb': 'stm32f091_c',
    'stm32f091rc': 'stm32f091_c',
    'stm32f091vb': 'stm32f091_c',
    'stm32f091vc': 'stm32f091_c',
    'stm32f098cc': 'stm32f098__',
    'stm32f098rc': 'stm32f098__',
    'stm32f098vc': 'stm32f098__',
}
mcu = meson.get_external_property('mcu')
if mcu not in mcus
  mcus_keys = mcus.keys()
  error(f'Please specify external property \'mcu\' with one of this values: @mcus_keys@')
endif
type = mcus[mcu]

type_lower = type.replace('_', 'x')
system_src = files(root / 'Source' / 'Templates' / 'system_stm32f0xx.c')
startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
type_define = '-D' + type.to_upper().replace('_', 'x')
pkg_define = '-DSTM32F0'  # This can be used with universal stm32 header 

lib = static_library(
  f'st_cmsis_@type_lower@', system_src, startup_src,
  build_by_default: false,
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  c_args: type_define,
  cpp_args: type_define,
)

st_cmsis_f0_type_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  compile_args: [type_define, pkg_define],
  link_with: lib,
)
st_cmsis_f0_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: st_cmsis_f0_type_dep,
  link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
)