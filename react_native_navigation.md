# ReactNavigation




> O [React Navigation](https://reactnavigation.org) Oferece lógica de roteamento e interface de usuário comuns em aplicativos móveis e PWAs, com navegação segura quanto a tipos, suporte nativo a links universais em dispositivos móveis e integração com o histórico do navegador na web.

 

## Instalando o React Navigation

###  Instalar os pacotes básicos para o funcionamento

```bash
npm install @react-navigation/native
npm install react-native-screens react-native-safe-area-context
```

| Pacote | Para que serve |
|---|---|
| `@react-navigation/native` | Núcleo da navegação |
| `react-native-screens` | Telas nativas, mais rápidas e com menos memória |
| `react-native-safe-area-context` | Respeita notch e barra de status |

### Conferir Instalação

```bash
npm ls @react-navigation/native
```
## Lógica de Navegação Estudadas

###  1. Stack

O Stack Navigator fornece uma forma de a sua aplicação fazer a transição entre ecrãs onde cada novo ecrã é colocado no topo de uma pilha.

```bash
npm install @react-navigation/stack
```

###  2. Native Stack

O Native Stack Navigator fornece uma forma de a sua aplicação fazer a transição entre ecrãs onde cada novo ecrã é colocado no topo de uma pilha.

```bash
npm install @react-navigation/native-stack
```


### 3. Drawer Navigation

O Drawer Navigator renderiza uma gaveta de navegação na lateral do ecrã que pode ser aberta e fechada através de gestos.

```bash
npm install @react-navigation/drawer
```


### 4. Bottom Tabs

Uma barra de separadores simples na parte inferior do ecrã que permite alternar entre diferentes rotas. As rotas são inicializadas lentamente - os seus componentes de ecrã não são montados até serem focados primeiro.

```bash
npm install @react-navigation/bottom-tabs
```


## Desenvolvimento

