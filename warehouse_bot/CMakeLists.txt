cmake_minimum_required(VERSION 3.8)
project(warehouse_bot)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclpy REQUIRED)

# gazebo dependencies
find_package(gazebo REQUIRED)
find_package(gazebo_ros REQUIRED)
find_package(gazebo_msgs REQUIRED)

install(DIRECTORY
  include
  launch
  worlds
  models
  urdf
  DESTINATION share/${PROJECT_NAME}
)


##############################
# Animated_box gazebo plugin
##############################
add_library (animated_box SHARED src/animated_box.cpp)
target_compile_options(animated_box PUBLIC "${GAZEBO_CXX_FLAGS}")
target_include_directories(animated_box PUBLIC ${GAZEBO_INCLUDE_DIRS})
target_link_directories(animated_box PUBLIC ${GAZEBO_LIBRARY_DIRS})

# Ensure plugin version compatibility
target_compile_definitions(animated_box PUBLIC "GAZEBO_MAJOR_VERSION=${GAZEBO_MAJOR_VERSION}")


install(TARGETS animated_box
  DESTINATION lib/${PROJECT_NAME})

##############################
# Gazebo resources
##############################
# Install gazebo world files
install(FILES
  worlds/my_world_2.world
  DESTINATION share/${PROJECT_NAME}/worlds/
)

# Install gazebo model files
install(DIRECTORY
  models
  DESTINATION share/${PROJECT_NAME}/
)

# Export dependencies
ament_export_dependencies(
  rosidl_default_runtime 
)


ament_package()
