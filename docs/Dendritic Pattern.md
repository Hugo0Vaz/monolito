# Dedritic Pattern

Existe um problema em criar configurações de Nix: existem muitas formas de
arquitetar um projeto usando módulos nix e quando falamos de **aspectos** da 
configuração (uma determinado *feature*), como por exemplo: *Hyprland*, esses
acabam necessitando de configurações nos níveis de sistema (NixOS, Darwin) e
configurações no nível de usuário (home-manager). Essa restrição acaba nos
fazendo em organizar os módulos de forma truncada e não muita lógica, uma vez
que a configuração de um comportamento está distribuída em vários módulos.

A solução? O padrão *dendritic*. A proposta desse padrão é que todo arquivo seja
um módulo do *flake-parts*

```nix aspect.nix
{
  flake.modules.nixos.basicPackages =
    { pkgs }:
    {
      environment.systemPackages =
        with pkgs;
        [
          # list of NixOS system packages
        ];
      # necessary configurations
    };

  flake.modules.darwin.basicPackages =
    { pkgs }:
    {
      environment.systemPackages = with pkgs;
      [
		# list of Darwin system packages
      ];
      # necessary configurations
    };

  flake.modules.homeManager.basicPackages =
    { pkgs }:
    {
      programs = 
        {
          # configuration of various programs
        };
    };
}
```
