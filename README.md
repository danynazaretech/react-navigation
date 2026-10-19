# React Navigation: 6 exemplos de navegadores

Projeto didático Expo + TypeScript com exemplos independentes de:

1. Stack (JS stack)
2. Native Stack
3. Drawer
4. Bottom Tabs
5. Native Bottom Tabs
6. Top Tabs

## Criar projeto limpo

```bash
npx create-expo-app@latest ReactNavigationDemo --template blank-typescript
cd ReactNavigationDemo
```

Copie `App.tsx`, `screens/` e `navigators/` deste pacote para a pasta do projeto.

## Instalar dependências base

```bash
npm install @react-navigation/native
npx expo install react-native-screens react-native-safe-area-context
```

## Instalar os navegadores

```bash
npm install @react-navigation/stack
npm install @react-navigation/native-stack
npm install @react-navigation/drawer
npm install @react-navigation/bottom-tabs
npm install @react-navigation/native-bottom-tabs
npm install @react-navigation/material-top-tabs
npx expo install react-native-gesture-handler react-native-reanimated react-native-pager-view
```

Siga também as instruções específicas e compatíveis com a versão do Expo SDK / React Navigation instalada:
- https://reactnavigation.org/docs/getting-started/
- https://reactnavigation.org/docs/stack-navigator/
- https://reactnavigation.org/docs/native-stack-navigator/
- https://reactnavigation.org/docs/drawer-navigator/
- https://reactnavigation.org/docs/bottom-tab-navigator/
- https://reactnavigation.org/docs/native-bottom-tab-navigator/
- https://reactnavigation.org/docs/material-top-tab-navigator/

> Observação: Native Bottom Tabs depende de versões compatíveis de React Navigation e `react-native-screens`; confira a documentação do seu SDK. No Android, a implementação nativa limita a barra a cinco abas.

## Executar

```bash
npx expo start
```

Na tela inicial do exemplo, selecione um navegador para experimentar.

## Conceitos comparados

- **Stack**: pilha com transições controladas pelo JavaScript.
- **Native Stack**: pilha baseada em componentes nativos do sistema.
- **Drawer**: menu lateral.
- **Bottom Tabs**: abas inferiores desenhadas pelo React Navigation.
- **Native Bottom Tabs**: abas inferiores com implementação nativa.
- **Top Tabs**: abas superiores, normalmente com gesto de deslizar entre páginas.


# 6. Exemplo de navegação com Top Tabs no React Native

## 1. Objetivo

Implementar uma navegação por abas posicionadas na parte superior da tela, utilizando a biblioteca React Navigation em um projeto React Native com Expo e TypeScript.

O navegador **Top Tabs** permite alternar entre diferentes telas selecionando abas localizadas na parte superior da interface. Dependendo da configuração, também é possível navegar entre elas deslizando o dedo horizontalmente.

## 2. Instalar a biblioteca

No terminal, dentro da pasta do projeto, execute:

```bash
npm install @react-navigation/material-top-tabs
npx expo install react-native-pager-view
```

O pacote `@react-navigation/material-top-tabs` implementa a navegação por abas superiores, enquanto `react-native-pager-view` fornece o mecanismo de paginação entre as telas.

## 3. Criar o arquivo do navegador

Dentro da pasta `navigators`, crie o arquivo:

```text
navigators/TopTabsExample.tsx
```

Adicione o seguinte código:

```tsx
import { NavigationContainer } from '@react-navigation/native';
import { createMaterialTopTabNavigator } from
  '@react-navigation/material-top-tabs';

import DemoScreen from '../screens/DemoScreen';

// Cria o navegador de abas superiores
const Tab = createMaterialTopTabNavigator();

export default function TopTabsExample() {
  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={{
          tabBarActiveTintColor: '#ffffff',
          tabBarInactiveTintColor: '#cccccc',
          tabBarStyle: {
            backgroundColor: '#2f9e41',
          },
          tabBarIndicatorStyle: {
            backgroundColor: '#ffffff',
          },
        }}
      >
        <Tab.Screen
          name="Inicio"
          component={DemoScreen}
          initialParams={{
            title: 'Início',
            description: 'Tela inicial do aplicativo',
          }}
        />

        <Tab.Screen
          name="Producao"
          component={DemoScreen}
          initialParams={{
            title: 'Produção',
            description: 'Informações sobre a produção de café',
          }}
        />

        <Tab.Screen
          name="Mercado"
          component={DemoScreen}
          initialParams={{
            title: 'Mercado',
            description: 'Informações sobre o mercado de café',
          }}
        />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

**Observação:** esse exemplo utiliza o componente `DemoScreen` compartilhado com os outros navegadores do projeto. Para exibir títulos e descrições diferentes, ele precisa ler esses valores de `route.params`.

## 4. Conferir o componente DemoScreen

No arquivo `screens/DemoScreen.tsx`, utilize uma implementação semelhante a esta:

```tsx
import { View, Text, StyleSheet } from 'react-native';
import { RouteProp, useRoute } from '@react-navigation/native';

type DemoParams = {
  title?: string;
  description?: string;
};

export default function DemoScreen() {
  const route = useRoute<RouteProp<Record<string, DemoParams>, string>>();

  const title = route.params?.title ?? 'Exemplo';
  const description =
    route.params?.description ?? 'Tela de demonstração';

  return (
    <View style={styles.container}>
      <Text style={styles.title}>{title}</Text>
      <Text style={styles.description}>{description}</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 24,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 12,
  },
  description: {
    fontSize: 16,
    textAlign: 'center',
  },
});
```

Se o projeto já possui um `DemoScreen` funcional, confira sua implementação antes de substituí-lo, para não afetar os demais exemplos.

## 5. Integrar o exemplo à tela inicial

No arquivo `App.tsx`, importe o navegador:

```tsx
import TopTabsExample from './navigators/TopTabsExample';

export default function App() {
  return <TopTabsExample />;
}
```

Esse código abre diretamente o exemplo Top Tabs. Se você quiser manter a tela inicial com os seis exemplos, registre `TopTabsExample` no mecanismo de seleção de navegadores já existente, em vez de substituir a tela inicial.

## 6. Executar o projeto

No terminal, execute:

```bash
npx expo start --web
```

Se o servidor já estiver funcionando, pare-o com `Ctrl + C` antes de reiniciar.

Para limpar o cache e iniciar novamente:

```bash
npx expo start --web --clear
```

## 7. Testar o resultado

Verifique os seguintes comportamentos:

- [ ] A aplicação abre sem erros.
- [ ] As abas Início, Produção e Mercado aparecem na parte superior.
- [ ] A aba selecionada fica visualmente destacada.
- [ ] Ao clicar em outra aba, o conteúdo da tela muda.
- [ ] O título e a descrição correspondem à aba selecionada.
- [ ] A navegação também funciona ao testar no dispositivo móvel compatível.

## 8. Compreender o código

| Elemento | Função |
|---|---|
| `NavigationContainer` | Gerencia o estado da navegação. |
| `createMaterialTopTabNavigator()` | Cria o navegador de abas superiores. |
| `Tab.Navigator` | Define e configura o conjunto de abas. |
| `Tab.Screen` | Registra cada tela navegável. |
| `component={DemoScreen}` | Define o componente exibido em cada aba. |
| `initialParams` | Fornece parâmetros iniciais para a tela. |
| `screenOptions` | Personaliza a aparência das abas. |

## 9. Desafio para os estudantes

Modifique o exemplo para criar três abas:

1. **Propriedades:** exibe o nome de uma propriedade rural.
2. **Produção:** apresenta a quantidade de sacas de café produzidas.
3. **Mercado:** apresenta o preço fictício da saca de café.

Personalize as cores, os títulos e as descrições. Depois, explique a diferença entre navegar por abas superiores e navegar por uma pilha de telas com Stack Navigator.
