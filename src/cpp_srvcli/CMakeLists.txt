cmake_minimum_required(VERSION 3.5)
project(cpp_srvcli)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(example_interfaces REQUIRED)
find_package(tutorial_interfaces REQUIRED)



add_executable(server src/add_two_ints_server.cpp)
ament_target_dependencies(server
  rclcpp example_interfaces)

add_executable(client src/add_two_ints_client.cpp)
ament_target_dependencies(client
  rclcpp example_interfaces)


add_executable(server2 src/add_two_ints_server2.cpp)
ament_target_dependencies(server2
  rclcpp tutorial_interfaces)

add_executable(client2 src/add_two_ints_client2.cpp)
ament_target_dependencies(client2
  rclcpp tutorial_interfaces)

  # find_package(rosidl_default_generators REQUIRED)

  # rosidl_generate_interfaces(${PROJECT_NAME}
  #   "msg/Num.msg"
  #   "srv/AddThreeInts.srv"
  # )

install(TARGETS
  server
  client
  server2
  client2

  DESTINATION lib/${PROJECT_NAME})

ament_package()