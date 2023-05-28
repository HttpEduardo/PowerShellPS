# PSClass

Este é um módulo do PowerShell para ajudar a determinar as dependências de classe e também para determinar a ordem que você precisa para importar as classes para o seu módulo.

## Instalar

Você pode instalar o PSClass da Galeria do PowerShell

```powershell
Módulo de instalação PSClass
```

## Uso

Todas as classes/enums precisam estar dentro de um diretório `/Classes` e referências a classes personalizadas referenciadas como `[ClassA]`. Os nomes das classes também devem corresponder ao nome do arquivo PowerShell - portanto, um `ClassA` *deve* estar dentro de um arquivo `ClassA.ps1`.

Por exemplo, se você tiver alguma classe `ClassA` e alguma outra classe `ClassB`, o PSClass verá `[ClassB]::Method()` e marcará que deve ser importado antes de `ClassA` automaticamente:

```powershell
classe classe A
{
     static [void] SomeMethod()
     {
         [ClasseB]::Método()
     }
}
```

Aqui, PSClass saberá que `ClassB` precisa ser importado antes de `ClassA`.

PSClass verá quaisquer linhas `[...]` e as usará para determinar a ordem em que as classes/enums devem ser importadas - assim como detectar dependências cíclicas.

> Classes/enums podem estar dentro de subdiretórios dentro do diretório `/Classes`.

## Funções

### Get-PSClassOrder

```powershell
Get-PSClassOrder [-Path <string>]
```

Quando executada, esta função irá procurar a pasta `/Classes` no `-Path` especificado (o padrão é o caminho atual).

Em seguida, ele retornará uma matriz dos caminhos de classe/enum na ordem precisa em que devem ser importados:

```powershell
Get-PSClassOrder | ForEach-Object { . $_ }
```

### Show-PSClassGraph

```powershell
Show-PSClassgraph [-Path <string>] [-Namespace <string>]
```

Retornará o gráfico de dependência completo das classes/enums ou apenas as dependências do namespace especificado. O namespace é o caminho para uma classe (sem extensão), e as barras são substituídas por pontos - como `Tools.Helpers` para uma classe chamada `Helpers` em `/Classes/Tools/Helpers.ps1`.

### Test-PSClassCyclic

```powershell
Test-PSClassCyclic [-Path <string>] [-Namespace <string>]
```

Irá testar dependências cíclicas em classes/enums. O namespace é o caminho para uma classe (sem extensão), e as barras são substituídas por pontos - como `Tools.Helpers` para uma classe chamada `Helpers` em `/Classes/Tools/Helpers.ps1`.
# PowerShellPS
