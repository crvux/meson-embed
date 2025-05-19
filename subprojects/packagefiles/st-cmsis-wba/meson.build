project('st-cmsis-wba', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.6.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-wba-1.6.0'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32wba50__', 
  'stm32wba52__', 
  'stm32wba54__', 
  'stm32wba55__', 
  'stm32wba5m__', 
  'stm32wba62__', 
  'stm32wba63__', 
  'stm32wba64__', 
  'stm32wba65__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32wbaxx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32WBA'  # This can be used with universal stm32 header 

  lib = static_library(
    f'st_cmsis_@type_lower@', system_src, startup_src,
    build_by_default: false,
    dependencies: [st_cmsis_core],
    include_directories: root / 'Include',
    c_args: type_define,
    cpp_args: type_define,
  )
  set_variable(f'st_cmsis_@type@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: [st_cmsis_core],
    include_directories: root / 'Include',
    compile_args: [type_define, pkg_define],
    link_with: lib,
  ))
endforeach

# Per MCU dependencies with default linker script
mcus = {
    'stm32wba50kg': 'stm32wba50__',
    'stm32wba52ce': 'stm32wba52__',
    'stm32wba52cg': 'stm32wba52__',
    'stm32wba52ke': 'stm32wba52__',
    'stm32wba52kg': 'stm32wba52__',
    'stm32wba54ce': 'stm32wba54__',
    'stm32wba54cg': 'stm32wba54__',
    'stm32wba54ke': 'stm32wba54__',
    'stm32wba54kg': 'stm32wba54__',
    'stm32wba55ce': 'stm32wba55__',
    'stm32wba55cg': 'stm32wba55__',
    'stm32wba55hg': 'stm32wba55__',
    'stm32wba55ue': 'stm32wba55__',
    'stm32wba55ug': 'stm32wba55__',
    'stm32wba62cg': 'stm32wba62__',
    'stm32wba62ci': 'stm32wba62__',
    'stm32wba62mg': 'stm32wba62__',
    'stm32wba62mi': 'stm32wba62__',
    'stm32wba62pg': 'stm32wba62__',
    'stm32wba62pi': 'stm32wba62__',
    'stm32wba63cg': 'stm32wba63__',
    'stm32wba63ci': 'stm32wba63__',
    'stm32wba64cg': 'stm32wba64__',
    'stm32wba64ci': 'stm32wba64__',
    'stm32wba65cg': 'stm32wba65__',
    'stm32wba65ci': 'stm32wba65__',
    'stm32wba65mg': 'stm32wba65__',
    'stm32wba65mi': 'stm32wba65__',
    'stm32wba65pg': 'stm32wba65__',
    'stm32wba65pi': 'stm32wba65__',
    'stm32wba65rg': 'stm32wba65__',
    'stm32wba65ri': 'stm32wba65__',
}
foreach mcu, type : mcus
  set_variable(f'st_cmsis_@mcu@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: get_variable(f'st_cmsis_@type@_dep'),
    link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
  ))
endforeach