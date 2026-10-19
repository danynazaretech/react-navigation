# React Native + React Navigation: do zero à navegação

Tutorial prático para estudantes. Vamos criar um projeto novo, instalar
as dependências, criar telas e construir Stack, Bottom Tabs, Top Tabs e
Drawer. Também vamos explicar o que é necessário para Native Bottom
Tabs.

## Objetivos

Ao terminar, você deverá conseguir:

-   preparar o ambiente com Node.js, VS Code e Expo;
-   criar um aplicativo React Native com TypeScript;
-   construir telas separadas;
-   navegar entre telas com Native Stack;
-   construir navegação por abas inferiores e superiores;
-   criar um menu lateral;
-   entender os requisitos específicos de Native Bottom Tabs.

------------------------------------------------------------------------

# Parte 1 --- Preparar o ambiente

## 1. Instalar Node.js

Acesse [nodejs.org](https://nodejs.org/) e instale a versão LTS.

O Node.js permite executar ferramentas JavaScript. O npm, que instala as
bibliotecas do projeto, é instalado junto com ele.

Abra um terminal e confira:

``` bash
node -v
npm -v
```

Se os dois comandos mostrarem números de versão, a instalação básica
está funcionando.

## 2. Instalar Visual Studio Code

Baixe o editor em
[code.visualstudio.com](https://code.visualstudio.com/).

Abra o VS Code e selecione **Terminal → Novo Terminal**. Vamos usar esse
terminal para executar os comandos.

## 3. Criar o projeto Expo com TypeScript

No terminal, execute:

``` bash
npx create-expo-app@latest NavegacaoAula --template blank-typescript
```

Entre na pasta:

``` bash
cd NavegacaoAula
```

Se o comando estiver disponível, abra a pasta atual no VS Code:

``` bash
code .
```

Caso `code .` não funcione, abra a pasta manualmente no VS Code usando
**Arquivo → Abrir Pasta**.

A pasta correta é aquela que contém `package.json`.

> O modelo `blank-typescript` cria um projeto inicial simples. Se o
> comando do template tiver mudado na versão do Expo instalada, consulte
> [Create a
> project](https://docs.expo.dev/get-started/create-a-project/).

------------------------------------------------------------------------

# Parte 2 --- Instalar React Navigation

## 4. Instalar o núcleo e o Native Stack

Dentro da pasta `NavegacaoAula`, execute:

``` bash
npm install @react-navigation/native @react-navigation/native-stack
```

Depois, instale as dependências nativas compatíveis com o Expo:

``` bash
npx expo install react-native-screens react-native-safe-area-context
```

-   `@react-navigation/native`: núcleo da navegação.
-   `@react-navigation/native-stack`: navegador de pilha com recursos
    nativos.
-   `react-native-screens`: integração com telas nativas.
-   `react-native-safe-area-context`: ajuda a respeitar as áreas seguras
    da tela.

## 5. Criar a primeira tela

Crie uma pasta chamada `screens`. Dentro dela, crie `HomeScreen.tsx`:

``` tsx
import { View, Text, StyleSheet } from 'react-native';

export default function HomeScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Minha primeira tela</Text>
      <Text style={styles.description}>
        Estou aprendendo React Navigation do zero!
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 24,
  },
  title: {
    fontSize: 26,
    fontWeight: 'bold',
    marginBottom: 12,
  },
  description: {
    fontSize: 16,
    textAlign: 'center',
  },
});
```

### Entendendo o código

-   `View`: contêiner visual.
-   `Text`: apresenta texto.
-   `StyleSheet.create()`: organiza os estilos.
-   `flex: 1`: ocupa o espaço disponível.
-   `justifyContent: 'center'`: centraliza os elementos no eixo
    principal.
-   `alignItems: 'center'`: centraliza os elementos no eixo transversal.
-   `export default`: permite importar a tela em outro arquivo.

## 6. Criar uma segunda tela

Na mesma pasta `screens`, crie `DetailsScreen.tsx`:

``` tsx
import { View, Text, StyleSheet } from 'react-native';

export default function DetailsScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Tela de detalhes</Text>
      <Text>Esta é a segunda tela do aplicativo.</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 24,
  },
  title: {
    fontSize: 26,
    fontWeight: 'bold',
    marginBottom: 12,
  },
});
```

Agora temos duas telas, mas ainda precisamos conectá-las por meio de um
navegador.

------------------------------------------------------------------------

# Parte 3 --- Construir Native Stack

## 7. Configurar as rotas no `App.tsx`

Abra `App.tsx`, substitua seu conteúdo por:

``` tsx
import { NavigationContainer } from '@react-navigation/native';
import {
  createNativeStackNavigator,
  type NativeStackScreenProps,
} from '@react-navigation/native-stack';

import { View, Text, Button, StyleSheet } from 'react-native';
import HomeScreen from './screens/HomeScreen';
import DetailsScreen from './screens/DetailsScreen';

type RootStackParamList = {
  Inicio: undefined;
  Detalhes: undefined;
};

const Stack = createNativeStackNavigator<RootStackParamList>();

type HomeProps = NativeStackScreenProps<RootStackParamList, 'Inicio'>;

function HomeWithButton({ navigation }: HomeProps) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Minha primeira tela</Text>
      <Text>Esta é a tela inicial do aplicativo.</Text>
      <Button
        title="Abrir detalhes"
        onPress={() => navigation.navigate('Detalhes')}
      />
    </View>
  );
}

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen
          name="Inicio"
          component={HomeWithButton}
          options={{ title: 'Página inicial' }}
        />

        <Stack.Screen
          name="Detalhes"
          component={DetailsScreen}
          options={{ title: 'Detalhes' }}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 24,
    gap: 16,
  },
  title: {
    fontSize: 26,
    fontWeight: 'bold',
  },
});
```

### O que cada parte faz?

-   `NavigationContainer`: gerencia o estado de navegação.
-   `createNativeStackNavigator`: cria um navegador em pilha.
-   `Stack.Navigator`: agrupa as rotas da pilha.
-   `Stack.Screen`: registra uma rota e o componente a ser exibido.
-   `name`: identifica a rota.
-   `component`: define o componente da tela.
-   `options.title`: define o título do cabeçalho.
-   `navigation.navigate('Detalhes')`: abre a rota `Detalhes`.

`RootStackParamList` informa ao TypeScript quais rotas existem e quais
parâmetros aceitam. Aqui, `undefined` significa que a rota não precisa
receber parâmetros.

## 8. Executar o aplicativo

Execute:

``` bash
npx expo start
```

Siga as instruções exibidas pelo Expo para abrir o aplicativo em um
dispositivo ou emulador. Toque em **Abrir detalhes**. O cabeçalho deve
permitir voltar para a tela anterior quando houver uma rota anterior na
pilha.

Documentação: [Native Stack
Navigator](https://reactnavigation.org/docs/native-stack-navigator/).

------------------------------------------------------------------------

# Parte 4 --- Criar telas reutilizáveis

Para os exemplos de abas e menu lateral, vamos criar telas simples.

## 9. Criar `ProductionScreen.tsx`

Em `screens/ProductionScreen.tsx`:

``` tsx
import { View, Text, StyleSheet } from 'react-native';

export default function ProductionScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Produção</Text>
      <Text>Esta é a área de produção.</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    gap: 12,
  },
  title: {
    fontSize: 26,
    fontWeight: 'bold',
  },
});
```

## 10. Criar `ProfileScreen.tsx`

Em `screens/ProfileScreen.tsx`:

``` tsx
import { View, Text, StyleSheet } from 'react-native';

export default function ProfileScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Perfil</Text>
      <Text>Esta é a área do perfil.</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    gap: 12,
  },
  title: {
    fontSize: 26,
    fontWeight: 'bold',
  },
});
```

## 11. Criar `DataScreen.tsx`

Em `screens/DataScreen.tsx`:

``` tsx
import { View, Text, StyleSheet } from 'react-native';

export default function DataScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Dados</Text>
      <Text>Informações detalhadas do aplicativo.</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    gap: 12,
  },
  title: {
    fontSize: 26,
    fontWeight: 'bold',
  },
});
```

## 12. Criar `AboutScreen.tsx`

Em `screens/AboutScreen.tsx`:

``` tsx
import { View, Text, StyleSheet } from 'react-native';

export default function AboutScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Sobre</Text>
      <Text>Aplicativo desenvolvido para a aula.</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    gap: 12,
  },
  title: {
    fontSize: 26,
    fontWeight: 'bold',
  },
});
```

------------------------------------------------------------------------

# Parte 5 --- Construir Bottom Tabs

## 13. Instalar a biblioteca

``` bash
npm install @react-navigation/bottom-tabs
```

## 14. Criar `navigators/BottomTabsNavigator.tsx`

Crie a pasta `navigators` e o arquivo `BottomTabsNavigator.tsx`:

``` tsx
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

import HomeScreen from '../screens/HomeScreen';
import ProductionScreen from '../screens/ProductionScreen';
import ProfileScreen from '../screens/ProfileScreen';

type TabParamList = {
  Inicio: undefined;
  Producao: undefined;
  Perfil: undefined;
};

const Tab = createBottomTabNavigator<TabParamList>();

export default function BottomTabsNavigator() {
  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={{
          headerShown: true,
          tabBarActiveTintColor: '#1565C0',
          tabBarInactiveTintColor: '#777777',
        }}
      >
        <Tab.Screen
          name="Inicio"
          component={HomeScreen}
          options={{ title: 'Início' }}
        />

        <Tab.Screen
          name="Producao"
          component={ProductionScreen}
          options={{ title: 'Produção' }}
        />

        <Tab.Screen
          name="Perfil"
          component={ProfileScreen}
          options={{ title: 'Perfil' }}
        />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

### Entenda a construção

1.  `createBottomTabNavigator()` cria o navegador de abas inferiores.
2.  `Tab.Navigator` define a barra de abas.
3.  Cada `Tab.Screen` registra uma aba.
4.  `name` é o identificador da rota.
5.  `component` aponta para o componente que será exibido.
6.  `options.title` define o título apresentado no cabeçalho.

## 15. Testar Bottom Tabs

Para testar esse navegador isoladamente, substitua temporariamente o
conteúdo de `App.tsx` por:

``` tsx
import BottomTabsNavigator from './navigators/BottomTabsNavigator';

export default function App() {
  return <BottomTabsNavigator />;
}
```

Salve e observe as abas Início, Produção e Perfil.

Documentação: [Bottom Tabs
Navigator](https://reactnavigation.org/docs/bottom-tab-navigator/).

------------------------------------------------------------------------

# Parte 6 --- Construir Top Tabs

## 16. Instalar as bibliotecas

``` bash
npm install @react-navigation/material-top-tabs
npx expo install react-native-pager-view
```

## 17. Criar `navigators/TopTabsNavigator.tsx`

``` tsx
import { NavigationContainer } from '@react-navigation/native';
import { createMaterialTopTabNavigator } from '@react-navigation/material-top-tabs';

import HomeScreen from '../screens/HomeScreen';
import DataScreen from '../screens/DataScreen';
import ProfileScreen from '../screens/ProfileScreen';

const Tab = createMaterialTopTabNavigator();

export default function TopTabsNavigator() {
  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={{
          tabBarIndicatorStyle: {
            backgroundColor: '#1565C0',
            height: 3,
          },
          tabBarLabelStyle: {
            fontWeight: 'bold',
          },
        }}
      >
        <Tab.Screen
          name="Inicio"
          component={HomeScreen}
          options={{ title: 'Início' }}
        />

        <Tab.Screen
          name="Dados"
          component={DataScreen}
        />

        <Tab.Screen
          name="Perfil"
          component={ProfileScreen}
        />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

Para testar, altere temporariamente `App.tsx`:

``` tsx
import TopTabsNavigator from './navigators/TopTabsNavigator';

export default function App() {
  return <TopTabsNavigator />;
}
```

Agora as abas aparecem na parte superior. Dependendo da configuração,
também é possível trocar de página deslizando horizontalmente.

Documentação: [Material Top Tabs
Navigator](https://reactnavigation.org/docs/material-top-tab-navigator/).

------------------------------------------------------------------------

# Parte 7 --- Construir Drawer

## 18. Instalar o Drawer e suas dependências

``` bash
npm install @react-navigation/drawer
npx expo install react-native-gesture-handler react-native-reanimated
```

Confira a documentação de instalação do React Navigation e do Expo para
confirmar os requisitos da versão instalada.

## 19. Criar `navigators/DrawerNavigator.tsx`

``` tsx
import { NavigationContainer } from '@react-navigation/native';
import { createDrawerNavigator } from '@react-navigation/drawer';

import HomeScreen from '../screens/HomeScreen';
import ProductionScreen from '../screens/ProductionScreen';
import AboutScreen from '../screens/AboutScreen';

const Drawer = createDrawerNavigator();

export default function DrawerNavigator() {
  return (
    <NavigationContainer>
      <Drawer.Navigator>
        <Drawer.Screen
          name="Inicio"
          component={HomeScreen}
          options={{ title: 'Início' }}
        />

        <Drawer.Screen
          name="Producao"
          component={ProductionScreen}
          options={{ title: 'Produção' }}
        />

        <Drawer.Screen
          name="Sobre"
          component={AboutScreen}
        />
      </Drawer.Navigator>
    </NavigationContainer>
  );
}
```

Para testar, altere temporariamente `App.tsx`:

``` tsx
import DrawerNavigator from './navigators/DrawerNavigator';

export default function App() {
  return <DrawerNavigator />;
}
```

Abra o menu lateral e selecione as opções.

Documentação: [Drawer
Navigator](https://reactnavigation.org/docs/drawer-navigator/).

------------------------------------------------------------------------

# Parte 8 --- Native Bottom Tabs

## 20. Entender a diferença

Bottom Tabs e Native Bottom Tabs não são necessariamente o mesmo
navegador. A versão comum utiliza `@react-navigation/bottom-tabs`. O
navegador nativo depende da API e dos requisitos de versão
disponibilizados pela documentação oficial.

Antes de instalar ou alterar o projeto:

1.  confira a versão do React Navigation;
2.  consulte [Native Bottom
    Tabs](https://reactnavigation.org/docs/native-bottom-tab-navigator/);
3.  confirme os requisitos do Expo SDK, React Native e
    `react-native-screens`;
4.  verifique se o ambiente de execução suporta o navegador escolhido.

Não basta importar `createNativeBottomTabNavigator` e depois instanciar
`createBottomTabNavigator`: isso continuaria criando o navegador
JavaScript. Também não presuma que um módulo nativo estará disponível no
Expo Go; alguns cenários exigem um development build.

------------------------------------------------------------------------

# Parte 9 --- Como organizar vários exemplos no mesmo aplicativo

Até aqui, substituímos temporariamente `App.tsx` para testar um
navegador por vez. Isso é didático no início, mas não é ideal para uma
demonstração completa.

Quando vários navegadores forem integrados em um aplicativo, o
recomendado é ter um único `NavigationContainer` na raiz e organizar os
fluxos usando navegadores aninhados ou um navegador principal. Não
coloque um `NavigationContainer` dentro de cada tela quando todos fazem
parte da mesma árvore de navegação.

Uma boa sequência de desenvolvimento é:

1.  implementar e testar cada navegador isoladamente;
2.  confirmar que as rotas e as telas funcionam;
3.  aprender como navegadores podem ser aninhados;
4.  montar um menu inicial que permita escolher os exemplos;
5.  testar novamente o fluxo completo.

------------------------------------------------------------------------

# Parte 10 --- Erros comuns

## Erro: módulo não encontrado

Confira se a biblioteca foi instalada na pasta que contém
`package.json`. Para dependências nativas, use `npx expo install` quando
recomendado pela documentação.

## Erro: tela não abre

Verifique se a rota foi registrada e se o nome usado em
`navigation.navigate('Detalhes')` corresponde exatamente ao nome em
`Screen`.

## Erro de TypeScript na navegação

Confira se o tipo das rotas corresponde ao nome do navegador e se os
parâmetros obrigatórios foram declarados corretamente.

## Abas não aparecem

Confirme que o navegador usa o componente correto, como `Tab.Navigator`,
e que suas telas estão registradas com `Tab.Screen`.

## Erro ao usar Drawer ou Top Tabs

Confira os requisitos de instalação das dependências nativas e as
versões compatíveis com o Expo SDK do projeto.

------------------------------------------------------------------------

# Parte 11 --- Exercícios para os alunos

1.  Altere o título da tela inicial.
2.  Crie uma terceira tela chamada `SettingsScreen.tsx`.
3.  Adicione uma rota para Configurações no Bottom Tabs.
4.  Mude a cor da aba ativa.
5.  Adicione uma aba no Top Tabs.
6.  Adicione uma opção chamada Ajuda ao Drawer.
7.  Crie um botão em Stack para navegar até a tela de detalhes.
8.  Explique a função de `NavigationContainer`, `Navigator` e `Screen`.
9.  Explique a diferença entre Stack e Native Stack.
10. Pesquise os requisitos necessários para Native Bottom Tabs no
    ambiente usado na aula.

------------------------------------------------------------------------

# Resumo

  -----------------------------------------------------------------------
  Navegador               Organização             Uso comum
  ----------------------- ----------------------- -----------------------
  Native Stack            Pilha de telas          Detalhes, formulários e
                                                  etapas

  Bottom Tabs             Abas inferiores         Seções principais
                                                  acessadas
                                                  frequentemente

  Top Tabs                Abas superiores         Categorias relacionadas

  Drawer                  Menu lateral            Aplicativos com várias
                                                  áreas

  Native Bottom Tabs      Abas inferiores nativas Quando a implementação
                                                  nativa é necessária e
                                                  suportada
  -----------------------------------------------------------------------

## Documentação oficial

-   [Criar um projeto
    Expo](https://docs.expo.dev/get-started/create-a-project/)
-   [React Navigation --- Getting
    Started](https://reactnavigation.org/docs/getting-started/)
-   [Native
    Stack](https://reactnavigation.org/docs/native-stack-navigator/)
-   [Bottom
    Tabs](https://reactnavigation.org/docs/bottom-tab-navigator/)
-   [Material Top
    Tabs](https://reactnavigation.org/docs/material-top-tab-navigator/)
-   [Drawer](https://reactnavigation.org/docs/drawer-navigator/)
-   [Native Bottom
    Tabs](https://reactnavigation.org/docs/native-bottom-tab-navigator/)
