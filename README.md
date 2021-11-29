# spring_boot_app

version: '1.0'
stages:
  - prepare
  - build
steps:
  main_clone:
    title: Cloning main repository...
    stage: prepare
    type: git-clone
    repo: 'codefresh-contrib/gradle-sample-app'
    revision: master
    git: github
  BuildingDockerImage:
    title: Building Docker Image
    stage: build
    type: build
    image_name: gradle-sample-app
    working_directory: ./
    tag: 'multi-stage'
    dockerfile: Dockerfile

    
    
    image: gradle:6.9.1-jdk8

stages:
  - build
  - test

cache:
  paths:
    - .gradle/wrapper
    - .gradle/caches

test:
  stage: test
  script:
    - echo "Gradle build started"
    - gradle check

build:
  stage: build
  script:
    - echo "Gradle build started"
    - gradle clean build
  artifacts:
    name: "app-snapshot"
    paths:
      - build/libs/*.jar
    expire_in: 1 week    

after_script:
  - echo "End Gradle CI"

