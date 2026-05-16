project('lsp-framework', 'cpp', version: '0.1', default_options: ['cpp_std=c++20'])

# 1. Build the generator
lspgen_exe = executable('lspgen',
  sources: [
    'lspgen/lspgen.cpp',
    'lsp/json/json.cpp'
  ],
  include_directories: include_directories('.'),
  native: true
)

# 2. Run the generator (output goes to meson.current_build_dir())
lsp_generated = custom_target('lspgen_run',
  output: ['types.h', 'messages.h', 'types.cpp'],
  command: ['sh', '-c', 'cd "$1" && "$2" "$3"', 'sh', '@OUTDIR@', lspgen_exe, join_paths(meson.current_source_dir(), 'lspgen', 'metaModel.json')]
)

# 3. Build liblsp
liblsp = static_library('lsp',
  sources: [
    'lsp/connection.cpp',
    'lsp/fileuri.cpp',
    'lsp/messagehandler.cpp',
    'lsp/process.cpp',
    'lsp/threadpool.cpp',
    'lsp/uri.cpp',
    'lsp/io/socket.cpp',
    'lsp/io/standardio.cpp',
    'lsp/json/json.cpp',
    'lsp/jsonrpc/jsonrpc.cpp',
    lsp_generated[2] # types.cpp
  ],
  include_directories: [
    include_directories('.')
  ],
  cpp_args: ['-Wno-sign-conversion', '-Wno-float-conversion']
)

# 4. Expose the dependency
lsp_dep = declare_dependency(
  link_with: liblsp,
  include_directories: [
    include_directories('.')
  ]
)
