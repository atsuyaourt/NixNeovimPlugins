# nixpkgs-vim-plugins

Nix flake of miscellaneous Vim/Neovim plugins.

## Description

This flake contains Nix packages of miscellaneous Vim/Neovim plugins.
Most of them are simply not provided by the official nixpkgs.
Some of them are provided but patched in this flake, or their version/revision
in the official nixpkgs is not up to date for my use case.

Packages are automatically updated twice per week using GitHub Actions.

Moreover, Neovim plugins listed in [awesome-neovim][0] are automatically generated by
parsing the README. Since these packages are automatically generated, some of them could
be broken due to lack of appropriate overrides (missing dependencies, build inputs, etc.).
So you should be careful if you want to use them.

## Usage

### In flake

The overlay simply adds extra Vim plugins into `pkgs.vimExtraPlugins`.
Use it as you normally do, like

```nix
{
  inputs = {
    flake-utils.url = "github:numtide/flake-utils";
    vim-plugins.url = "github:m15a/nixpkgs-vim-plugins";
  };
  outputs = { self, nixpkgs, flake-utils, vim-plugins, ... }:
  flake-utils.lib.eachDefaultSystem (system:
  let
    pkgs = import nixpkgs {
      inherit system;
      overlays = [ vim-plugins.overlay ];
    };
  in {
    packages = {
      my-neovim = pkgs.neovim.override {
        configure = {
          packages.example = with pkgs.vimExtraPlugins; {
            start = [
              lspactions
              vim-hy
            ];
          };
        };
      };
    };
  });
}
```

### In legacy Nix

It is handy to use `builtins.getFlake`, which was [introduced in Nix 2.4][1]. For example,

```nix
with import <nixpkgs> {
  overlays = [
    (builtins.getFlake "github:m15a/nixpkgs-vim-plugins").overlay
  ];
};
```

For Nix <2.4, use `builtins.fetchTarball` instead.

```nix
with import <nixpkgs> {
  overlays = [
    (import (builtins.fetchTarball {
      url = "https://github.com/m15a/nixpkgs-vim-plugins/archive/main.tar.gz";
    })).overlay
  ];
};
```

### Via NUR

You can also use it via [NUR][2] at `nur.repos.m15a.vimExtraPlugins`, see [the package list][3].

[0]: https://github.com/rockerBOO/awesome-neovim
[1]: https://nixos.org/manual/nix/stable/release-notes/rl-2.4.html?highlight=builtins.getFlake#other-features
[2]: https://nur.nix-community.org/
[3]: https://nur.nix-community.org/repos/m15a/

## Available Vim/Neovim plugins

| Plugin owner/repo | Recent commit | Nix package name |
| :- | :- | :- |
| [AckslD/nvim-revJ.lua](https://github.com/AckslD/nvim-revJ.lua) | 2021-09-12 | `nvim-revJ-lua` |
| [AllenDang/nvim-expand-expr](https://github.com/AllenDang/nvim-expand-expr) | 2021-08-14 | `nvim-expand-expr` |
| [CRAG666/code_runner.nvim](https://github.com/CRAG666/code_runner.nvim) | 2021-11-02 | `code-runner-nvim` |
| [David-Kunz/cmp-npm](https://github.com/David-Kunz/cmp-npm) | 2021-10-27 | `cmp-npm` |
| [David-Kunz/jester](https://github.com/David-Kunz/jester) | 2021-12-02 | `jester` |
| [FrenzyExists/aquarium-vim](https://github.com/FrenzyExists/aquarium-vim) | 2021-12-12 | `aquarium-vim` |
| [JoosepAlviste/nvim-ts-context-commentstring](https://github.com/JoosepAlviste/nvim-ts-context-commentstring) | 2021-12-13 | `nvim-ts-context-commentstring` |
| [L3MON4D3/LuaSnip](https://github.com/L3MON4D3/LuaSnip) | 2021-12-17 | `LuaSnip` |
| [LoricAndre/OneTerm.nvim](https://github.com/LoricAndre/OneTerm.nvim) | 2021-07-30 | `OneTerm-nvim` |
| [Mangeshrex/uwu.vim](https://github.com/Mangeshrex/uwu.vim) | 2021-12-10 | `uwu-vim` |
| [mcauley-penney/tidy.nvim](https://github.com/mcauley-penney/tidy.nvim) | 2021-11-22 | `tidy-nvim` |
| [Mofiqul/vscode.nvim](https://github.com/Mofiqul/vscode.nvim) | 2021-12-18 | `vscode-nvim` |
| [MordechaiHadad/nvim-papadark](https://github.com/MordechaiHadad/nvim-papadark) | 2021-10-30 | `nvim-papadark` |
| [NFrid/due.nvim](https://github.com/NFrid/due.nvim) | 2021-07-04 | `due-nvim` |
| [NTBBloodbath/cheovim](https://github.com/NTBBloodbath/cheovim) | 2021-08-25 | `cheovim` |
| [NTBBloodbath/doom-one.nvim](https://github.com/NTBBloodbath/doom-one.nvim) | 2021-10-20 | `doom-one-nvim` |
| [NTBBloodbath/galaxyline.nvim](https://github.com/NTBBloodbath/galaxyline.nvim) | 2021-12-16 | `galaxyline-nvim` |
| [NTBBloodbath/rest.nvim](https://github.com/NTBBloodbath/rest.nvim) | 2021-12-17 | `rest-nvim` |
| [PHSix/nvim-hybrid](https://github.com/PHSix/nvim-hybrid) | 2021-07-31 | `nvim-hybrid` |
| [Pocco81/AbbrevMan.nvim](https://github.com/Pocco81/AbbrevMan.nvim) | 2021-07-15 | `AbbrevMan-nvim` |
| [Pocco81/AutoSave.nvim](https://github.com/Pocco81/AutoSave.nvim) | 2021-12-14 | `AutoSave-nvim` |
| [Pocco81/DAPInstall.nvim](https://github.com/Pocco81/DAPInstall.nvim) | 2021-11-05 | `DAPInstall-nvim` |
| [Pocco81/HighStr.nvim](https://github.com/Pocco81/HighStr.nvim) | 2021-08-17 | `HighStr-nvim` |
| [RRethy/nvim-treesitter-textsubjects](https://github.com/RRethy/nvim-treesitter-textsubjects) | 2021-11-28 | `nvim-treesitter-textsubjects` |
| [RishabhRD/gruvy](https://github.com/RishabhRD/gruvy) | 2021-12-01 | `gruvy` |
| [RishabhRD/lspactions](https://github.com/RishabhRD/lspactions) | 2021-11-15 | `lspactions` |
| [RishabhRD/nvim-rdark](https://github.com/RishabhRD/nvim-rdark) | 2020-12-25 | `nvim-rdark` |
| [SmiteshP/nvim-gps](https://github.com/SmiteshP/nvim-gps) | 2021-11-14 | `nvim-gps` |
| [Th3Whit3Wolf/onebuddy](https://github.com/Th3Whit3Wolf/onebuddy) | 2021-04-01 | `onebuddy` |
| [Th3Whit3Wolf/space-nvim](https://github.com/Th3Whit3Wolf/space-nvim) | 2021-07-08 | `space-nvim` |
| [ThePrimeagen/vim-be-good](https://github.com/ThePrimeagen/vim-be-good) | 2021-03-16 | `vim-be-good` |
| [TimUntersberger/neofs](https://github.com/TimUntersberger/neofs) | 2021-11-02 | `neofs` |
| [Xuyuanp/yanil](https://github.com/Xuyuanp/yanil) | 2021-10-20 | `yanil` |
| [abecodes/tabout.nvim](https://github.com/abecodes/tabout.nvim) | 2021-12-15 | `tabout-nvim` |
| [adelarsq/neoline.vim](https://github.com/adelarsq/neoline.vim) | 2021-08-26 | `neoline-vim` |
| [adisen99/apprentice.nvim](https://github.com/adisen99/apprentice.nvim) | 2021-12-12 | `apprentice-nvim` |
| [adisen99/codeschool.nvim](https://github.com/adisen99/codeschool.nvim) | 2021-12-12 | `codeschool-nvim` |
| [ahmedkhalf/project.nvim](https://github.com/ahmedkhalf/project.nvim) | 2021-11-06 | `project-nvim` |
| [akinsho/dependency-assist.nvim](https://github.com/akinsho/dependency-assist.nvim) | 2021-11-11 | `dependency-assist-nvim` |
| [akinsho/flutter-tools.nvim](https://github.com/akinsho/flutter-tools.nvim) | 2021-12-02 | `flutter-tools-nvim` |
| [akinsho/toggleterm.nvim](https://github.com/akinsho/toggleterm.nvim) | 2021-11-24 | `toggleterm-nvim` |
| [alec-gibson/nvim-tetris](https://github.com/alec-gibson/nvim-tetris) | 2021-06-28 | `nvim-tetris` |
| [alexaandru/nvim-lspupdate](https://github.com/alexaandru/nvim-lspupdate) | 2021-11-29 | `nvim-lspupdate` |
| [aliou/bats.vim](https://github.com/aliou/bats.vim) | 2021-01-10 | `bats-vim` |
| [amirrezaask/fuzzy.nvim](https://github.com/amirrezaask/fuzzy.nvim) | 2021-05-13 | `fuzzy-nvim` |
| [andersevenrud/cmp-tmux](https://github.com/andersevenrud/cmp-tmux) | 2021-12-20 | `cmp-tmux` |
| [andersevenrud/nordic.nvim](https://github.com/andersevenrud/nordic.nvim) | 2021-12-20 | `nordic-nvim` |
| [anott03/nvim-lspinstall](https://github.com/anott03/nvim-lspinstall) | 2021-07-23 | `nvim-lspinstall` |
| [beauwilliams/statusline.lua](https://github.com/beauwilliams/statusline.lua) | 2021-09-28 | `statusline-lua` |
| [bfredl/nvim-luadev](https://github.com/bfredl/nvim-luadev) | 2021-11-30 | `nvim-luadev` |
| [bkegley/gloombuddy](https://github.com/bkegley/gloombuddy) | 2021-04-16 | `gloombuddy` |
| [blackCauldron7/surround.nvim](https://github.com/blackCauldron7/surround.nvim) | 2021-12-16 | `surround-nvim` |
| [bluz71/vim-moonfly-colors](https://github.com/bluz71/vim-moonfly-colors) | 2021-12-09 | `vim-moonfly-colors` |
| [bluz71/vim-nightfly-guicolors](https://github.com/bluz71/vim-nightfly-guicolors) | 2021-12-09 | `vim-nightfly-guicolors` |
| [catppuccin/nvim](https://github.com/catppuccin/nvim) | 2021-12-14 | `catppuccin` |
| [clojure-vim/jazz.nvim](https://github.com/clojure-vim/jazz.nvim) | 2019-04-30 | `jazz-nvim` |
| [code-biscuits/nvim-biscuits](https://github.com/code-biscuits/nvim-biscuits) | 2021-11-12 | `nvim-biscuits` |
| [crispgm/nvim-go](https://github.com/crispgm/nvim-go) | 2021-12-15 | `nvim-go` |
| [crispgm/nvim-tabline](https://github.com/crispgm/nvim-tabline) | 2021-10-18 | `nvim-tabline` |
| [crispgm/telescope-heading.nvim](https://github.com/crispgm/telescope-heading.nvim) | 2021-12-02 | `telescope-heading-nvim` |
| [danymat/neogen](https://github.com/danymat/neogen) | 2021-12-08 | `neogen` |
| [datwaft/bubbly.nvim](https://github.com/datwaft/bubbly.nvim) | 2021-11-15 | `bubbly-nvim` |
| [davidgranstrom/nvim-markdown-preview](https://github.com/davidgranstrom/nvim-markdown-preview) | 2021-12-07 | `nvim-markdown-preview` |
| [davidgranstrom/scnvim](https://github.com/davidgranstrom/scnvim) | 2021-12-01 | `scnvim` |
| [dcampos/nvim-snippy](https://github.com/dcampos/nvim-snippy) | 2021-12-18 | `nvim-snippy` |
| [dkarter/bullets.vim](https://github.com/dkarter/bullets.vim) | 2021-06-18 | `bullets-vim` |
| [echasnovski/mini.nvim](https://github.com/echasnovski/mini.nvim) | 2021-12-19 | `mini-nvim` |
| [edolphin-ydf/goimpl.nvim](https://github.com/edolphin-ydf/goimpl.nvim) | 2021-12-07 | `goimpl-nvim` |
| [ekickx/clipboard-image.nvim](https://github.com/ekickx/clipboard-image.nvim) | 2021-12-07 | `clipboard-image-nvim` |
| [ellisonleao/glow.nvim](https://github.com/ellisonleao/glow.nvim) | 2021-12-14 | `glow-nvim` |
| [ethanholz/nvim-lastplace](https://github.com/ethanholz/nvim-lastplace) | 2021-10-15 | `nvim-lastplace` |
| [feline-nvim/feline.nvim](https://github.com/feline-nvim/feline.nvim) [branch: `develop`] | 2021-12-19 | `feline-nvim-develop` |
| [filipdutescu/renamer.nvim](https://github.com/filipdutescu/renamer.nvim) | 2021-12-19 | `renamer-nvim` |
| [folke/lua-dev.nvim](https://github.com/folke/lua-dev.nvim) | 2021-12-10 | `lua-dev-nvim` |
| [folke/zen-mode.nvim](https://github.com/folke/zen-mode.nvim) | 2021-11-07 | `zen-mode-nvim` |
| [fuenor/JpFormat.vim](https://github.com/fuenor/JpFormat.vim) | 2019-07-12 | `JpFormat-vim` |
| [gbprod/cutlass.nvim](https://github.com/gbprod/cutlass.nvim) | 2021-12-13 | `cutlass-nvim` |
| [gennaro-tedesco/nvim-commaround](https://github.com/gennaro-tedesco/nvim-commaround) | 2021-12-16 | `nvim-commaround` |
| [glacambre/firenvim](https://github.com/glacambre/firenvim) | 2021-12-16 | `firenvim` |
| [glepnir/indent-guides.nvim](https://github.com/glepnir/indent-guides.nvim) | 2021-03-26 | `indent-guides-nvim` |
| [glepnir/prodoc.nvim](https://github.com/glepnir/prodoc.nvim) | 2021-01-23 | `prodoc-nvim` |
| [goolord/alpha-nvim](https://github.com/goolord/alpha-nvim) | 2021-12-18 | `alpha-nvim` |
| [gwatcha/reaper-keys](https://github.com/gwatcha/reaper-keys) | 2021-08-23 | `reaper-keys` |
| [h-hg/fcitx.nvim](https://github.com/h-hg/fcitx.nvim) | 2021-12-11 | `fcitx-nvim` |
| [henriquehbr/nvim-startup.lua](https://github.com/henriquehbr/nvim-startup.lua) | 2021-10-10 | `nvim-startup-lua` |
| [houtsnip/vim-emacscommandline](https://github.com/houtsnip/vim-emacscommandline) | 2017-11-24 | `vim-emacscommandline` |
| [hylang/vim-hy](https://github.com/hylang/vim-hy) | 2021-05-20 | `vim-hy` |
| [ibhagwan/fzf-lua](https://github.com/ibhagwan/fzf-lua) | 2021-12-19 | `fzf-lua` |
| [inkch/vim-fish](https://github.com/inkch/vim-fish) | 2021-05-21 | `vim-fish-inkch` |
| [is0n/fm-nvim](https://github.com/is0n/fm-nvim) | 2021-12-18 | `fm-nvim` |
| [is0n/jaq-nvim](https://github.com/is0n/jaq-nvim) | 2021-12-18 | `jaq-nvim` |
| [ishan9299/modus-theme-vim](https://github.com/ishan9299/modus-theme-vim) | 2021-11-09 | `modus-theme-vim` |
| [jakewvincent/mkdnflow.nvim](https://github.com/jakewvincent/mkdnflow.nvim) | 2021-11-10 | `mkdnflow-nvim` |
| [jakewvincent/texmagic.nvim](https://github.com/jakewvincent/texmagic.nvim) | 2021-09-01 | `texmagic-nvim` |
| [jameshiew/nvim-magic](https://github.com/jameshiew/nvim-magic) | 2021-10-09 | `nvim-magic` |
| [jbyuki/instant.nvim](https://github.com/jbyuki/instant.nvim) | 2021-10-26 | `instant-nvim` |
| [jbyuki/nabla.nvim](https://github.com/jbyuki/nabla.nvim) | 2021-11-08 | `nabla-nvim` |
| [jbyuki/one-small-step-for-vimkind](https://github.com/jbyuki/one-small-step-for-vimkind) | 2021-10-26 | `one-small-step-for-vimkind` |
| [jghauser/auto-pandoc.nvim](https://github.com/jghauser/auto-pandoc.nvim) | 2021-06-14 | `auto-pandoc-nvim` |
| [jghauser/follow-md-links.nvim](https://github.com/jghauser/follow-md-links.nvim) | 2021-06-12 | `follow-md-links-nvim` |
| [jghauser/kitty-runner.nvim](https://github.com/jghauser/kitty-runner.nvim) | 2021-06-16 | `kitty-runner-nvim` |
| [jghauser/mkdir.nvim](https://github.com/jghauser/mkdir.nvim) | 2021-06-20 | `mkdir-nvim` |
| [jim-at-jibba/ariake-vim-colors](https://github.com/jim-at-jibba/ariake-vim-colors) | 2021-02-23 | `ariake-vim-colors` |
| [johann2357/nvim-smartbufs](https://github.com/johann2357/nvim-smartbufs) | 2021-06-14 | `nvim-smartbufs` |
| [jose-elias-alvarez/null-ls.nvim](https://github.com/jose-elias-alvarez/null-ls.nvim) | 2021-12-19 | `null-ls-nvim` |
| [jubnzv/mdeval.nvim](https://github.com/jubnzv/mdeval.nvim) | 2021-10-02 | `mdeval-nvim` |
| [jubnzv/virtual-types.nvim](https://github.com/jubnzv/virtual-types.nvim) | 2021-11-06 | `virtual-types-nvim` |
| [jvgrootveld/telescope-zoxide](https://github.com/jvgrootveld/telescope-zoxide) | 2021-10-21 | `telescope-zoxide` |
| [kana/vim-textobj-indent](https://github.com/kana/vim-textobj-indent) | 2013-01-18 | `vim-textobj-indent` |
| [kazhala/close-buffers.nvim](https://github.com/kazhala/close-buffers.nvim) | 2021-11-14 | `close-buffers-nvim` |
| [kdheepak/monochrome.nvim](https://github.com/kdheepak/monochrome.nvim) | 2021-07-14 | `monochrome-nvim` |
| [konapun/vacuumline.nvim](https://github.com/konapun/vacuumline.nvim) | 2021-08-11 | `vacuumline-nvim` |
| [nvim-orgmode/orgmode](https://github.com/nvim-orgmode/orgmode) | 2021-12-18 | `orgmode` |
| [kvrohit/substrata.nvim](https://github.com/kvrohit/substrata.nvim) | 2021-12-07 | `substrata-nvim` |
| [kyazdani42/blue-moon](https://github.com/kyazdani42/blue-moon) | 2021-12-05 | `blue-moon` |
| [ldelossa/calltree.nvim](https://github.com/ldelossa/calltree.nvim) | 2021-12-12 | `calltree-nvim` |
| [ldelossa/vimdark](https://github.com/ldelossa/vimdark) | 2021-11-24 | `vimdark` |
| [lewis6991/spellsitter.nvim](https://github.com/lewis6991/spellsitter.nvim) | 2021-12-18 | `spellsitter-nvim` |
| [lourenci/github-colors](https://github.com/lourenci/github-colors) | 2021-10-04 | `github-colors` |
| [lukas-reineke/cmp-rg](https://github.com/lukas-reineke/cmp-rg) | 2021-12-03 | `cmp-rg` |
| [lukas-reineke/format.nvim](https://github.com/lukas-reineke/format.nvim) | 2021-12-12 | `format-nvim` |
| [luukvbaal/nnn.nvim](https://github.com/luukvbaal/nnn.nvim) | 2021-12-02 | `nnn-nvim` |
| [m00qek/plugin-template.nvim](https://github.com/m00qek/plugin-template.nvim) | 2021-04-24 | `plugin-template-nvim` |
| [madskjeldgaard/reaper-nvim](https://github.com/madskjeldgaard/reaper-nvim) | 2021-01-29 | `reaper-nvim` |
| [marko-cerovac/material.nvim](https://github.com/marko-cerovac/material.nvim) | 2021-12-01 | `material-nvim` |
| [matbme/JABS.nvim](https://github.com/matbme/JABS.nvim) | 2021-09-22 | `JABS-nvim` |
| [mcchrish/zenbones.nvim](https://github.com/mcchrish/zenbones.nvim) | 2021-12-18 | `zenbones-nvim` |
| [mfussenegger/nvim-treehopper](https://github.com/mfussenegger/nvim-treehopper) | 2021-12-20 | `nvim-treehopper` |
| [michaelb/sniprun](https://github.com/michaelb/sniprun) | 2021-12-19 | `sniprun` |
| [mizlan/iswap.nvim](https://github.com/mizlan/iswap.nvim) | 2021-11-20 | `iswap-nvim` |
| [mjlbach/babelfish.nvim](https://github.com/mjlbach/babelfish.nvim) | 2021-09-14 | `babelfish-nvim` |
| [mnacamura/iron.nvim](https://github.com/mnacamura/iron.nvim) | 2021-12-19 | `iron-nvim-mnacamura` |
| [mnacamura/nvim-srcerite](https://github.com/mnacamura/nvim-srcerite) | 2021-12-17 | `nvim-srcerite` |
| [mnacamura/vim-fennel-syntax](https://github.com/mnacamura/vim-fennel-syntax) | 2021-07-08 | `vim-fennel-syntax` |
| [mnacamura/vim-r7rs-syntax](https://github.com/mnacamura/vim-r7rs-syntax) | 2021-07-09 | `vim-r7rs-syntax` |
| [monaqa/dial.nvim](https://github.com/monaqa/dial.nvim) | 2021-03-22 | `dial-nvim` |
| [monkoose/matchparen.nvim](https://github.com/monkoose/matchparen.nvim) | 2021-12-20 | `matchparen-nvim` |
| [ms-jpq/chadtree](https://github.com/ms-jpq/chadtree) | 2021-12-20 | `chadtree` |
| [ms-jpq/coq_nvim](https://github.com/ms-jpq/coq_nvim) | 2021-12-20 | `coq-nvim` |
| [nanotee/nvim-lsp-basics](https://github.com/nanotee/nvim-lsp-basics) | 2021-05-16 | `nvim-lsp-basics` |
| [nanotee/sqls.nvim](https://github.com/nanotee/sqls.nvim) | 2021-11-01 | `sqls-nvim` |
| [navarasu/onedark.nvim](https://github.com/navarasu/onedark.nvim) | 2021-11-30 | `onedark-nvim` |
| [nekonako/xresources-nvim](https://github.com/nekonako/xresources-nvim) | 2021-11-23 | `xresources-nvim` |
| [nikvdp/neomux](https://github.com/nikvdp/neomux) | 2021-12-15 | `neomux` |
| [noib3/nvim-cokeline](https://github.com/noib3/nvim-cokeline) | 2021-12-19 | `nvim-cokeline` |
| [norcalli/nvim-base16.lua](https://github.com/norcalli/nvim-base16.lua) | 2019-10-16 | `nvim-base16-lua` |
| [notomo/cmdbuf.nvim](https://github.com/notomo/cmdbuf.nvim) | 2021-09-19 | `cmdbuf-nvim` |
| [notomo/gesture.nvim](https://github.com/notomo/gesture.nvim) | 2021-08-15 | `gesture-nvim` |
| [novakne/kosmikoa.nvim](https://github.com/novakne/kosmikoa.nvim) | 2021-11-19 | `kosmikoa-nvim` |
| [numToStr/Comment.nvim](https://github.com/numToStr/Comment.nvim) | 2021-12-20 | `Comment-nvim` |
| [nvim-telescope/telescope-bibtex.nvim](https://github.com/nvim-telescope/telescope-bibtex.nvim) | 2021-11-29 | `telescope-bibtex-nvim` |
| [nxvu699134/vn-night.nvim](https://github.com/nxvu699134/vn-night.nvim) | 2021-07-24 | `vn-night-nvim` |
| [ojroques/nvim-hardline](https://github.com/ojroques/nvim-hardline) | 2021-12-10 | `nvim-hardline` |
| [ojroques/nvim-lspfuzzy](https://github.com/ojroques/nvim-lspfuzzy) | 2021-11-08 | `nvim-lspfuzzy` |
| [petertriho/cmp-git](https://github.com/petertriho/cmp-git) | 2021-11-29 | `cmp-git` |
| [pianocomposer321/consolation.nvim](https://github.com/pianocomposer321/consolation.nvim) | 2021-09-01 | `consolation-nvim` |
| [pianocomposer321/yabs.nvim](https://github.com/pianocomposer321/yabs.nvim) | 2021-12-11 | `yabs-nvim` |
| [projekt0n/github-nvim-theme](https://github.com/projekt0n/github-nvim-theme) | 2021-12-18 | `github-nvim-theme` |
| [pwntester/codeql.nvim](https://github.com/pwntester/codeql.nvim) | 2021-12-08 | `codeql-nvim` |
| [pwntester/octo.nvim](https://github.com/pwntester/octo.nvim) | 2021-12-05 | `octo-nvim` |
| [quangnguyen30192/cmp-nvim-ultisnips](https://github.com/quangnguyen30192/cmp-nvim-ultisnips) | 2021-12-17 | `cmp-nvim-ultisnips` |
| [rafaelsq/nvim-goc.lua](https://github.com/rafaelsq/nvim-goc.lua) | 2021-11-23 | `nvim-goc-lua` |
| [rafcamlet/nvim-luapad](https://github.com/rafcamlet/nvim-luapad) | 2021-08-15 | `nvim-luapad` |
| [rafcamlet/tabline-framework.nvim](https://github.com/rafcamlet/tabline-framework.nvim) | 2021-11-28 | `tabline-framework-nvim` |
| [raimon49/requirements.txt.vim](https://github.com/raimon49/requirements.txt.vim) | 2021-12-04 | `requirements-txt-vim` |
| [ray-x/go.nvim](https://github.com/ray-x/go.nvim) | 2021-12-18 | `go-nvim` |
| [ray-x/navigator.lua](https://github.com/ray-x/navigator.lua) | 2021-12-19 | `navigator-lua` |
| [rhysd/vim-gfm-syntax](https://github.com/rhysd/vim-gfm-syntax) | 2021-10-04 | `vim-gfm-syntax` |
| [rktjmp/highlight-current-n.nvim](https://github.com/rktjmp/highlight-current-n.nvim) | 2021-10-26 | `highlight-current-n-nvim` |
| [rktjmp/hotpot.nvim](https://github.com/rktjmp/hotpot.nvim) | 2021-11-25 | `hotpot-nvim` |
| [rktjmp/paperplanes.nvim](https://github.com/rktjmp/paperplanes.nvim) | 2021-08-14 | `paperplanes-nvim` |
| [rmehri01/onenord.nvim](https://github.com/rmehri01/onenord.nvim) | 2021-12-16 | `onenord-nvim` |
| [rockerBOO/boo-colorscheme-nvim](https://github.com/rockerBOO/boo-colorscheme-nvim) | 2021-12-08 | `boo-colorscheme-nvim` |
| [rose-pine/neovim](https://github.com/rose-pine/neovim) | 2021-12-17 | `rose-pine` |
| [s1n7ax/nvim-comment-frame](https://github.com/s1n7ax/nvim-comment-frame) | 2021-09-14 | `nvim-comment-frame` |
| [s1n7ax/nvim-terminal](https://github.com/s1n7ax/nvim-terminal) | 2021-11-12 | `nvim-terminal` |
| [sQVe/sort.nvim](https://github.com/sQVe/sort.nvim) | 2021-12-14 | `sort-nvim` |
| [saifulapm/chartoggle.nvim](https://github.com/saifulapm/chartoggle.nvim) | 2021-10-25 | `chartoggle-nvim` |
| [sainnhe/everforest](https://github.com/sainnhe/everforest) | 2021-12-17 | `everforest` |
| [savq/melange](https://github.com/savq/melange) | 2021-11-16 | `melange` |
| [savq/paq-nvim](https://github.com/savq/paq-nvim) | 2021-12-18 | `paq-nvim` |
| [seandewar/nvimesweeper](https://github.com/seandewar/nvimesweeper) | 2021-09-07 | `nvimesweeper` |
| [shaeinst/roshnivim-cs](https://github.com/shaeinst/roshnivim-cs) | 2021-12-18 | `roshnivim-cs` |
| [shaunsingh/moonlight.nvim](https://github.com/shaunsingh/moonlight.nvim) | 2021-05-16 | `moonlight-nvim` |
| [stevearc/dressing.nvim](https://github.com/stevearc/dressing.nvim) | 2021-12-19 | `dressing-nvim` |
| [stevearc/gkeep.nvim](https://github.com/stevearc/gkeep.nvim) | 2021-11-17 | `gkeep-nvim` |
| [stevearc/qf_helper.nvim](https://github.com/stevearc/qf_helper.nvim) | 2021-12-01 | `qf-helper-nvim` |
| [svermeulen/vim-yoink](https://github.com/svermeulen/vim-yoink) | 2021-09-15 | `vim-yoink` |
| [svermeulen/vimpeccable](https://github.com/svermeulen/vimpeccable) | 2021-09-25 | `vimpeccable` |
| [tamago324/nlsp-settings.nvim](https://github.com/tamago324/nlsp-settings.nvim) | 2021-12-20 | `nlsp-settings-nvim` |
| [tami5/lspsaga.nvim](https://github.com/tami5/lspsaga.nvim) | 2021-12-19 | `lspsaga-nvim` |
| [tamton-aquib/staline.nvim](https://github.com/tamton-aquib/staline.nvim) | 2021-12-13 | `staline-nvim` |
| [tanvirtin/monokai.nvim](https://github.com/tanvirtin/monokai.nvim) | 2021-10-06 | `monokai-nvim` |
| [tanvirtin/vgit.nvim](https://github.com/tanvirtin/vgit.nvim) | 2021-12-12 | `vgit-nvim` |
| [terrortylor/nvim-comment](https://github.com/terrortylor/nvim-comment) | 2021-08-01 | `nvim-comment` |
| [theniceboy/nvim-deus](https://github.com/theniceboy/nvim-deus) | 2021-08-26 | `nvim-deus` |
| [tjdevries/astronauta.nvim](https://github.com/tjdevries/astronauta.nvim) | 2021-11-09 | `astronauta-nvim` |
| [tjdevries/express_line.nvim](https://github.com/tjdevries/express_line.nvim) | 2021-12-01 | `express-line-nvim` |
| [tjdevries/gruvbuddy.nvim](https://github.com/tjdevries/gruvbuddy.nvim) | 2021-04-15 | `gruvbuddy-nvim` |
| [tjdevries/vlog.nvim](https://github.com/tjdevries/vlog.nvim) | 2020-08-04 | `vlog-nvim` |
| [tveskag/nvim-blame-line](https://github.com/tveskag/nvim-blame-line) | 2021-07-27 | `nvim-blame-line` |
| [nvim-neorg/neorg](https://github.com/nvim-neorg/neorg) | 2021-12-19 | `neorg` |
| [williamboman/nvim-lsp-installer](https://github.com/williamboman/nvim-lsp-installer) | 2021-12-19 | `nvim-lsp-installer` |
| [windwp/nvim-projectconfig](https://github.com/windwp/nvim-projectconfig) | 2021-11-10 | `nvim-projectconfig` |
| [nvim-pack/nvim-spectre](https://github.com/nvim-pack/nvim-spectre) | 2021-12-11 | `nvim-spectre` |
| [windwp/nvim-ts-autotag](https://github.com/windwp/nvim-ts-autotag) | 2021-12-19 | `nvim-ts-autotag` |
| [windwp/windline.nvim](https://github.com/windwp/windline.nvim) | 2021-12-05 | `windline-nvim` |
| [winston0410/commented.nvim](https://github.com/winston0410/commented.nvim) | 2021-11-14 | `commented-nvim` |
| [xiyaowong/nvim-cursorword](https://github.com/xiyaowong/nvim-cursorword) | 2021-12-16 | `nvim-cursorword` |
| [xiyaowong/nvim-transparent](https://github.com/xiyaowong/nvim-transparent) | 2021-11-28 | `nvim-transparent` |
| [yashguptaz/calvera-dark.nvim](https://github.com/yashguptaz/calvera-dark.nvim) | 2021-08-13 | `calvera-dark-nvim` |
| [yonlu/omni.vim](https://github.com/yonlu/omni.vim) | 2021-02-20 | `omni-vim` |
| [gitlab:yorickpeterse/nvim-pqf](https://gitlab.com/yorickpeterse/nvim-pqf) | 2021-10-29 | `nvim-pqf` |

## Contribution

### How to add a new plugin

#### 1. Add a new entry to `manifest.txt`

In `manifest.txt`, an entry is specified by one of the following forms:

- `owner/repo`: a GitHub repo with default branch (typically `main` or `master`).
- `github:owner/repo`: again, a GitHub repo with default branch.
- `gitlab:owner/repo`: a GitLab repo with default branch.
- `owner/repo:branch`: a GitHub repo with a specific `branch`.
- `github:owner/repo:branch`: again, a GitHub repo with a specific `branch`.
- `gitlab:owner/repo:branch`: a GitLab repo with a specific `branch`.
- `owner/repo::rename`: a GitHub repo with default branch, renamed to `rename`.
    - Example: `xxx/yyy::zzz`'s package name will be renamed to `zzz` in place of `yyy`.
- `github:owner/repo::rename`: again, a GitHub repo with default branch, renamed to `rename`.
- `gitlab:owner/repo::rename`: a GitLab repo with default branch, renamed to `rename`.
- `owner/repo:branch:rename`: a GitHub repo with a specific `branch`, renamed to `rename`.
- `github:owner/repo:branch:rename`: again, a GitHub repo with a specific `branch`, renamed to `rename`.
- `gitlab:owner/repo:branch:rename`: a GitLab repo with a specific `branch`, renamed to `rename`.

After adding your entry, run:

```
nix run .#update-vim-plugins -- lint
```

So that entries are sorted and duplicated ones are removed.

#### 2. Update Nix expression and README

Next, run this:

```
nix run .#update-vim-plugins
```

After that, `pkgs/vim-plugins.nix` and the plugin list in `README.md` are updated.

#### 3. Override your plugin derivation in `overrides.nix`

In `overrides.nix`, you see something like

```nix
  {
    # ...

    lspactions = super.lspactions.overrideAttrs (_: {
      dependencies = with final.vimPlugins; [
        plenary-nvim
        popup-nvim
        self.astronauta-nvim
      ];
    });

    # ...
  }
```

Add your overrides here if needed.

#### 4. Create a Pull Request if you like

Anyone is welcome to add another plugin to this repo.
Feel free to create a PR with your new plugins!
In that case, make sure you commit only `manifest.txt` and `overrides.nix` if changed.
Other files will be updated by GitHub Action.

## License

[MIT](LICENSE)
