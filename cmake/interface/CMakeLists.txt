project(${project_name}-interface)

# Interface
if(LINUX_GCC OR LINUX_CLANG OR WIN64_GCC OR WIN64_CLANG)
  set(gcc_like TRUE)	# indicates gcc-like compiler
else()
  set(gcc_like FALSE)
endif()

set(compile_opts
  $<$<BOOL:${gcc_like}>:-Wall -Wextra -Werror=return-type $<$<NOT:$<CONFIG:Debug>>:-Werror>>
  $<$<BOOL:${WIN64_MSBUILD}>:/MP $<$<NOT:$<CONFIG:Debug>>:/WX>>
)

add_library(${project_name}-interface INTERFACE)
add_library(${project_name}::interface ALIAS ${project_name}-interface)
target_compile_definitions(${project_name}-interface INTERFACE $<$<BOOL:${MSVC_RUNTIME}>:WIN32_LEAN_AND_MEAN NOMINMAX _CRT_SECURE_NO_WARNINGS>)
target_compile_options(${project_name}-interface INTERFACE ${compile_opts})
target_compile_features(${project_name}-interface INTERFACE ${cxx_standard})
