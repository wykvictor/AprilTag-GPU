file(GLOB_RECURSE LIBRARY_SOURCE_FILES "*.cc" "*.cu")

if (CUDA_FOUND)
# use cuda_add_library or cuda_add_executable
  cuda_add_library(apriltag SHARED ${LIBRARY_SOURCE_FILES})
else()
  add_library(apriltag SHARED ${LIBRARY_SOURCE_FILES})
endif()
target_link_libraries(apriltag ${DEPENDENCIES})
