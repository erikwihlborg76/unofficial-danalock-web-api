# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]


## [2.0.3] - 2024-02-25

## Changed

- Changed how actions are identified to make it compatible with Nodered installations running in HA OS + HA Supervised.

## [2.0.2] - 2024-02-25

## Changed

- Refactured and improved logging.
- Changed the endpoint for getting the state of the lock to `http://<node-red-host>:1880/danalock/<your lock>/state`.
- Add fallback option for users who need to set Danalock username + password in the flow.

## [2.0.1] - 2023-12-30

### Fixed

- Logging in the "poll section" didn´t work properly.

## [2.0.0] - 2023-12-30

### Changed

- Complete rewrite of the Node-RED flow to make it easier to maintain, and to benefit from new capabilities introduced in Node-RED 3.x such as link nodes. 

### Added

- Added logging using Node-RED standard logging capability.

### Removed

- Removed dependencies to 3rd party nodes for Oauth authentication.