project('st-cmsis-f2', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '2.2.6',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-f2-2.2.6'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32f205__', 
  'stm32f215__', 
  'stm32f207__', 
  'stm32f217__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32f2xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32F2'  # This can be used with universal stm32 header 

  lib = static_library(
    f'st_cmsis_@type_lower@', system_src, startup_src,
    build_by_default: false,
    dependencies: [st_cmsis_core],
    include_directories: root / 'Include',
    c_args: type_define,
    cpp_args: type_define,
  )
  set_variable(f'st_cmsis_@type_lower@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: [st_cmsis_core],
    include_directories: root / 'Include',
    compile_args: [type_define, pkg_define],
    link_with: lib,
  ))
endforeach

# Per MCU dependencies with default linker script
mcus = {
    'stm32f205rb': ['stm32f205__', 128, 64],
    'stm32f205rc': ['stm32f205__', 256, 96],
    'stm32f205re': ['stm32f205__', 512, 128],
    'stm32f205rf': ['stm32f205__', 768, 128],
    'stm32f205rg': ['stm32f205__', 1024, 128],
    'stm32f205vb': ['stm32f205__', 128, 64],
    'stm32f205vc': ['stm32f205__', 256, 96],
    'stm32f205ve': ['stm32f205__', 512, 128],
    'stm32f205vf': ['stm32f205__', 768, 128],
    'stm32f205vg': ['stm32f205__', 1024, 128],
    'stm32f205zc': ['stm32f205__', 256, 96],
    'stm32f205ze': ['stm32f205__', 512, 128],
    'stm32f205zf': ['stm32f205__', 768, 128],
    'stm32f205zg': ['stm32f205__', 1024, 128],
    'stm32f207ic': ['stm32f207__', 256, 128],
    'stm32f207ie': ['stm32f207__', 512, 128],
    'stm32f207if': ['stm32f207__', 768, 128],
    'stm32f207ig': ['stm32f207__', 1024, 128],
    'stm32f207vc': ['stm32f207__', 256, 128],
    'stm32f207ve': ['stm32f207__', 512, 128],
    'stm32f207vf': ['stm32f207__', 768, 128],
    'stm32f207vg': ['stm32f207__', 1024, 128],
    'stm32f207zc': ['stm32f207__', 256, 128],
    'stm32f207ze': ['stm32f207__', 512, 128],
    'stm32f207zf': ['stm32f207__', 768, 128],
    'stm32f207zg': ['stm32f207__', 1024, 128],
    'stm32f215re': ['stm32f215__', 512, 128],
    'stm32f215rg': ['stm32f215__', 1024, 128],
    'stm32f215ve': ['stm32f215__', 512, 128],
    'stm32f215vg': ['stm32f215__', 1024, 128],
    'stm32f215ze': ['stm32f215__', 512, 128],
    'stm32f215zg': ['stm32f215__', 1024, 128],
    'stm32f217ie': ['stm32f217__', 512, 128],
    'stm32f217ig': ['stm32f217__', 1024, 128],
    'stm32f217ve': ['stm32f217__', 512, 128],
    'stm32f217vg': ['stm32f217__', 1024, 128],
    'stm32f217ze': ['stm32f217__', 512, 128],
    'stm32f217zg': ['stm32f217__', 1024, 128],
}
foreach mcu, opts : mcus
  ld_default_script_name = f'@mcu@.ld'
  configure_file(input : 'default.ld.in',
    output : ld_default_script_name,
    configuration : {
      'flash_size_kb': opts[1],
      'ram_size_kb': opts[2],
  })
  ld_default_script = ['-Xlinker', '--default-script=' + meson.project_build_root() / ld_default_script_name]

  compile_args = []
  if get_option('use_hal')
    compile_args += '-DUSE_HAL_DRIVER'
  endif

  type_lower = opts[0].replace('_', 'x')
  set_variable(f'st_cmsis_@mcu@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: get_variable(f'st_cmsis_@type_lower@_dep'),
    compile_args: compile_args,
    link_args: ld_default_script,
  ))
endforeach