# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] - 2026-03-18

### Added

- `caddy_packages` variable for explicit package management in caddy role
- `caddy_sites_enabled_directory` variable to support modular Caddy configuration
- Package installation task to caddy role (`ansible.builtin.apt` module)
- Sites-enabled directory creation task in caddy role
- Package installation task to fail2ban role
- Explicit package configuration variables to keycloak role (`keycloak_packages`, `keycloak_packages_install_recommends`)
- New handlers in keycloak role (reload systemd user daemon, restart services)
- `kc-caddy.task` Caddyfile template to keycloak role for per-realm Caddy configuration
- Caddy configuration task to keycloak role (`caddy.yaml`)
- User creation task to keycloak role (`create_user.yaml`) - encapsulates user and lingering setup
- Keycloak-specific systemd and lingering configuration in keycloak role
- Import directive mechanism in Caddyfile for loading modular configuration from sites-enabled directory

### Changed

- **BREAKING**: Removed `base` role - its functionality distributed to individual roles (caddy, fail2ban, keycloak)
- **BREAKING**: Variable naming: `caddy_realms` renamed to `keycloak_caddy_realms` in playbook variables
- **BREAKING**: Removed `caddy_default_hostname` variable
- **BREAKING**: Removed `caddy_keycloak_endpoint` variable
- **BREAKING**: fail2ban keycloak jail now disabled by default (`fail2ban_keycloak_jail_enabled: false`)
- **BREAKING**: Role execution order in vagrant playbook - caddy role now runs before keycloak role
- Caddyfile template refactored to use import mechanism instead of inline realm configuration
- Package management restructured: each role now manages its own dependencies
- User creation and systemd lingering setup moved from base role to keycloak role
- Removed unnecessary `become: true` directives from caddy tasks (handled at role level)
- Caddy tasks simplified with updated structuring (package install, directory setup, configuration, service management)

### Removed

- `roles/base/` directory entirely (defaults, handlers, tasks)
- `base_packages` variable
- `base_packages_install_recommends` variable
- `base_username` variable
- `base_uid_min` and `base_uid_max` variables
- `caddy_default_hostname` variable
- `caddy_realms` variable (use `keycloak_caddy_realms` instead)
- `caddy_keycloak_endpoint` variable
- Inline realm configuration from Caddyfile template
- `base` role reference from vagrant playbook
- Dependency on base role for all other roles

### Fixed

- Caddy systemd service management now properly handled without redundant become directives
- Keycloak-specific systemd and user setup properly isolated to keycloak role

[2.0.0]: https://github.com/sebatec-eu/keycloak-collection/releases/tag/v2.0.0
