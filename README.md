# Design tokens sample

## Настройка

- Настроить интеграцию с Tokens Studio https://docs.tokens.studio/sync/github 
- Для билда использовать
```bash
yarn transform:dodo \
yarn transform:drinkit \
yarn build
```
- Опубликовать пакет

## Использование

```ts
// colors.ts

import { drinkit, dodo } from '@difuks/kiosk-tokens-sample';

import { TThemeTokenedProps } from 'shared/ui/Theme/types';

export const drinkitColors = {
	'bg-primary': drinkit.BackgroundPrimary,
	'bg-card-ingredient': drinkit.BackgroundArtIngredient,
	'bg-alpha-4': drinkit.BackgroundAlpha4,
	'bg-negative': drinkit.BackgroundNegative,
	'bg-card': drinkit.BackgroundCard,
	'bg-card-sticky': drinkit.BackgroundCartSticky,
	'bg-overlay': drinkit.BackgroundOverlay,

	'control-button-secondary': drinkit.ControlsButtonSecondary,
	'control-button-primary': drinkit.ControlsButtonPrimary,
	'control-counter-minus': drinkit.ControlsCounterMinus,
	'control-counter-plus': drinkit.ControlsCounterPlus,
	'control-toggle-selected': drinkit.ControlsToggleSelected,
	'control-toggle-unselected': drinkit.ControlsToggleUnselected,

	'text-primary': drinkit.TextPrimary,
	'text-on-color': drinkit.TextContrast,
	'text-secondary': drinkit.TextSecondary,
	'text-negative': drinkit.TextNegative,

	brand: drinkit.ColorsBrand,

	black: drinkit.ColorsBlack,
	white: drinkit.ColorsWhite,
};

export const pizzaColors = {
    'bg-primary': dodo.BackgroundPrimary,
    'bg-card-ingredient': dodo.BackgroundArtIngredient,
    'bg-alpha-4': dodo.BackgroundAlpha4,
    'bg-negative': dodo.BackgroundNegative,
    'bg-card': dodo.BackgroundCard,
    'bg-card-sticky': dodo.BackgroundCartSticky,
    'bg-overlay': dodo.BackgroundOverlay,
    
    'control-button-secondary': dodo.ControlsButtonSecondary,
    'control-button-primary': dodo.ControlsButtonPrimary,
    'control-counter-minus': dodo.ControlsCounterMinus,
    'control-counter-plus': dodo.ControlsCounterPlus,
    'control-toggle-selected': dodo.ControlsToggleSelected,
    'control-toggle-unselected': dodo.ControlsToggleUnselected,
    
    'text-primary': dodo.TextPrimary,
    'text-on-color': dodo.TextContrast,
    'text-secondary': dodo.TextSecondary,
    'text-negative': dodo.TextNegative,
    
    brand: dodo.ColorsBrand,
    
    black: dodo.ColorsBlack,
    white: dodo.ColorsWhite,
};

export type TColor = keyof typeof drinkitColors;

export const getColor = (color: TColor) => (props: TThemeTokenedProps) => props.theme.colors[color];

```

```ts
// tokens.ts

import { drinkitColors, pizzaColors } from 'shared/ui/Theme/tokens/color';
import { drinkitCurrency, pizzaCurrency } from 'shared/ui/Theme/tokens/currency';
import { drinkitText, pizzaText } from 'shared/ui/Theme/tokens/text';

export const drinkitTheme = {
  colors: drinkitColors,
  text: drinkitText,
  currency: drinkitCurrency,
  brand: `drinkit`,
};

export const pizzaTheme = {
  colors: pizzaColors,
  text: pizzaText,
  currency: pizzaCurrency,
  brand: `pizza`,
};

export type TTheme = typeof drinkitTheme;

```

```tsx
// Theme.tsx

import { FC, ReactNode, useMemo } from 'react';
import { useSearchParams } from 'react-router-dom';
import { DefaultTheme, ThemeProvider } from 'styled-components';

import { drinkitTheme, pizzaTheme } from './tokens/theme';

export type TBrand = 'drinkit' | 'pizza' | 'doner';

const getTheme = (brand: TBrand): DefaultTheme => {
  switch (brand) {
    case `pizza`:
      return pizzaTheme;

    case `drinkit`:
    default:
      return drinkitTheme;
  }
};

interface IProps {
  children: ReactNode;
}

export const Theme: FC<IProps> = ({ children }) => {
  const [searchParams] = useSearchParams();
  const brand = searchParams.get(`brand`);
  const theme = useMemo(() => getTheme(brand as TBrand), [brand]);

  return (
    <ThemeProvider theme={theme}>
      {children}
    </ThemeProvider>
  );
};
```
