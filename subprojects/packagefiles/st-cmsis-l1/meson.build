project('st-cmsis-l1', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '2.3.4',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-l1-2.3.4'

mcus = {
    'stm32l100c6': 'stm32l100_b',
    'stm32l100c6a': 'stm32l100_ba',
    'stm32l100r8': 'stm32l100_b',
    'stm32l100r8a': 'stm32l100_ba',
    'stm32l100rb': 'stm32l100_b',
    'stm32l100rba': 'stm32l100_ba',
    'stm32l100rc': 'stm32l100_c',
    'stm32l151c6': 'stm32l151_b',
    'stm32l151c6a': 'stm32l151_ba',
    'stm32l151c8': 'stm32l151_b',
    'stm32l151c8a': 'stm32l151_ba',
    'stm32l151cb': 'stm32l151_b',
    'stm32l151cba': 'stm32l151_ba',
    'stm32l151cc': 'stm32l151_c',
    'stm32l151qc': 'stm32l151_c',
    'stm32l151qd': 'stm32l151_d',
    'stm32l151qe': 'stm32l151_e',
    'stm32l151r6': 'stm32l151_b',
    'stm32l151r6a': 'stm32l151_ba',
    'stm32l151r8': 'stm32l151_b',
    'stm32l151r8a': 'stm32l151_ba',
    'stm32l151rb': 'stm32l151_b',
    'stm32l151rba': 'stm32l151_ba',
    'stm32l151rc': 'stm32l151_c',
    'stm32l151rca': 'stm32l151_ca',
    'stm32l151rd': 'stm32l151_d',
    'stm32l151re': 'stm32l151_e',
    'stm32l151uc': 'stm32l151_c',
    'stm32l151v8': 'stm32l151_b',
    'stm32l151v8a': 'stm32l151_ba',
    'stm32l151vb': 'stm32l151_b',
    'stm32l151vba': 'stm32l151_ba',
    'stm32l151vc': 'stm32l151_c',
    'stm32l151vca': 'stm32l151_ca',
    'stm32l151vd': 'stm32l151_d',
    'stm32l151vdx': 'stm32l151_dx',
    'stm32l151ve': 'stm32l151_e',
    'stm32l151zc': 'stm32l151_c',
    'stm32l151zd': 'stm32l151_d',
    'stm32l151ze': 'stm32l151_e',
    'stm32l152c6': 'stm32l152_b',
    'stm32l152c6a': 'stm32l152_ba',
    'stm32l152c8': 'stm32l152_b',
    'stm32l152c8a': 'stm32l152_ba',
    'stm32l152cb': 'stm32l152_b',
    'stm32l152cba': 'stm32l152_ba',
    'stm32l152cc': 'stm32l152_c',
    'stm32l152qc': 'stm32l152_c',
    'stm32l152qd': 'stm32l152_d',
    'stm32l152qe': 'stm32l152_e',
    'stm32l152r6': 'stm32l152_b',
    'stm32l152r6a': 'stm32l152_ba',
    'stm32l152r8': 'stm32l152_b',
    'stm32l152r8a': 'stm32l152_ba',
    'stm32l152rb': 'stm32l152_b',
    'stm32l152rba': 'stm32l152_ba',
    'stm32l152rc': 'stm32l152_c',
    'stm32l152rca': 'stm32l152_ca',
    'stm32l152rd': 'stm32l152_d',
    'stm32l152re': 'stm32l152_e',
    'stm32l152uc': 'stm32l152_c',
    'stm32l152v8': 'stm32l152_b',
    'stm32l152v8a': 'stm32l152_ba',
    'stm32l152vb': 'stm32l152_b',
    'stm32l152vba': 'stm32l152_ba',
    'stm32l152vc': 'stm32l152_c',
    'stm32l152vca': 'stm32l152_ca',
    'stm32l152vd': 'stm32l152_d',
    'stm32l152vdx': 'stm32l152_dx',
    'stm32l152ve': 'stm32l152_e',
    'stm32l152zc': 'stm32l152_c',
    'stm32l152zd': 'stm32l152_d',
    'stm32l152ze': 'stm32l152_e',
    'stm32l162qc': 'stm32l162_c',
    'stm32l162qd': 'stm32l162_d',
    'stm32l162rc': 'stm32l162_c',
    'stm32l162rca': 'stm32l162_ca',
    'stm32l162rd': 'stm32l162_d',
    'stm32l162re': 'stm32l162_e',
    'stm32l162vc': 'stm32l162_c',
    'stm32l162vca': 'stm32l162_ca',
    'stm32l162vd': 'stm32l162_d',
    'stm32l162vdx': 'stm32l162_dx',
    'stm32l162ve': 'stm32l162_e',
    'stm32l162zc': 'stm32l162_c',
    'stm32l162zd': 'stm32l162_d',
    'stm32l162ze': 'stm32l162_e',
}
mcu = meson.get_external_property('mcu')
if mcu not in mcus
  mcus_keys = mcus.keys()
  error(f'Please specify external property \'mcu\' with one of this values: @mcus_keys@')
endif
type = mcus[mcu]

type_lower = type.replace('_', 'x')
system_src = files(root / 'Source' / 'Templates' / 'system_stm32l1xx.c')
startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
type_define = '-D' + type.to_upper().replace('_', 'x')
pkg_define = '-DSTM32L1'  # This can be used with universal stm32 header 

lib = static_library(
  f'st_cmsis_@type_lower@', system_src, startup_src,
  build_by_default: false,
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  c_args: type_define,
  cpp_args: type_define,
)

st_cmsis_l1_type_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  compile_args: [type_define, pkg_define],
  link_with: lib,
)
st_cmsis_l1_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: st_cmsis_l1_type_dep,
  link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
)