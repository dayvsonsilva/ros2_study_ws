cmake_minimum_required(VERSION 3.8)
project(learning_tf2_cpp)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(rclcpp REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(turtlesim REQUIRED)

find_package(message_filters REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)


find_file(TF2_CPP_HEADERS
  NAMES tf2_geometry_msgs.hpp
  PATHS ${tf2_geometry_msgs_INCLUDE_DIRS}
  NO_CACHE
  PATH_SUFFIXES tf2_geometry_msgs
)

add_executable(static_turtle_tf2_broadcaster src/static_turtle_tf2_broadcaster.cpp)
ament_target_dependencies(
   static_turtle_tf2_broadcaster
   geometry_msgs
   rclcpp
   tf2
   tf2_ros
   turtlesim
)

add_executable(turtle_tf2_broadcaster src/turtle_tf2_broadcaster.cpp)
ament_target_dependencies(
    turtle_tf2_broadcaster
    geometry_msgs
    rclcpp
    tf2
    tf2_ros
    turtlesim
)
add_executable(turtle_tf2_listener src/turtle_tf2_listener.cpp)
ament_target_dependencies(
    turtle_tf2_listener
    geometry_msgs
    rclcpp
    tf2
    tf2_ros
    turtlesim
)
add_executable(fixed_frame_tf2_broadcaster src/fixed_frame_tf2_broadcaster.cpp)
ament_target_dependencies(
   fixed_frame_tf2_broadcaster
    geometry_msgs
    rclcpp
    tf2
    tf2_ros
    turtlesim
)


add_executable(dynamic_frame_tf2_broadcaster src/dynamic_frame_tf2_broadcaster.cpp)
ament_target_dependencies(
   dynamic_frame_tf2_broadcaster
   geometry_msgs
   rclcpp
   tf2
   tf2_ros
   turtlesim
)

add_executable(turtle_tf2_listener_debug src/turtle_tf2_listener_debug.cpp)
ament_target_dependencies(
   turtle_tf2_listener_debug
   geometry_msgs
   rclcpp
   tf2
   tf2_ros
   turtlesim
)

add_executable(turtle_tf2_message_filter src/turtle_tf2_message_filter.cpp)
ament_target_dependencies(
  turtle_tf2_message_filter
  geometry_msgs
  message_filters
  rclcpp
  tf2
  tf2_geometry_msgs
  tf2_ros
)

install(TARGETS
   static_turtle_tf2_broadcaster
   DESTINATION lib/${PROJECT_NAME})

install(TARGETS
   turtle_tf2_broadcaster
   DESTINATION lib/${PROJECT_NAME})

install(TARGETS
   turtle_tf2_listener
   DESTINATION lib/${PROJECT_NAME})

install(DIRECTORY
   launch
   DESTINATION share/${PROJECT_NAME}/
 )

 
 install(TARGETS
   fixed_frame_tf2_broadcaster
   DESTINATION lib/${PROJECT_NAME}
)


install(TARGETS
   dynamic_frame_tf2_broadcaster
   DESTINATION lib/${PROJECT_NAME}
)


install(TARGETS
   turtle_tf2_listener_debug
   DESTINATION lib/${PROJECT_NAME}
)

install(TARGETS
  turtle_tf2_message_filter
  DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
