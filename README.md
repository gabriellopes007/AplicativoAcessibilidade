# Céu Acessível — Accessible Astronomy App for Android

**Céu Acessível** ("Accessible Sky") is an Android app that makes astronomy content available to people with visual impairments. It combines a glossary, sky charts, descriptions of celestial objects and weather information, all designed to work with screen readers such as TalkBack.

> The app interface and content are written in **Brazilian Portuguese**. This README is in English to make the project easier to understand for a wider audience.

---

## Table of contents

- [Features](#features)
- [How the app is organized](#how-the-app-is-organized)
- [Home screen design](#home-screen-design)
- [Accessibility approach](#accessibility-approach)
- [Project structure](#project-structure)
- [Tech stack](#tech-stack)
- [Getting started](#getting-started)
- [Known limitations](#known-limitations)
- [Credits](#credits)

---

## Features

| Section | What it offers |
| --- | --- |
| **Glossary** | Astronomy terms organized alphabetically across ten letter-group screens (`ABC`, `DEF`, `GHI`, `JKL`, `MNO`, `PQR`, `ST`, `UV`, `WX`, `YZ`). |
| **Sky chart** | Monthly sky charts (March to June), with the March chart available for the North, East, West and South directions. |
| **Astronomical objects** | Four categories: nebulae, constellations, planets (Solar System) and the Moon / natural satellites, each with definitions and image descriptions. |
| **Weather information** | A page from `tempo.ufes.br` loaded inside a `WebView`. |
| **Side menu** | About the app, interface description, about the GOA observatory group, and contact links (Facebook and website). |

---

## How the app is organized

The app is a tree of screens (Android `Activity` classes). The home screen opens four main sections, and a side menu gives access to information about the app and its team.

![Navigation map of the app](docs/images/navigation-map.svg)

How navigation works in the code:

- Every button calls a small `onClick` method that starts the next screen with an `Intent`, for example:

  ```java
  public void proximaTela(View view){
      Intent it = new Intent(this, Main2Activity.class);
      startActivity(it);
  }
  ```

- `Main2Activity` to `Main5Activity` are the section menus (Glossary, Sky chart, Objects, Weather).
- Leaf screens (letters, months, planets, etc.) show text and images, and each has a "Voltar" (Back) button.
- The weather screen enables JavaScript in a `WebView` and loads `http://tempo.ufes.br:40000/tempo/index.html`.

---

## Home screen design

The home screen (`MainActivity`) is built from three layouts:

| Layout file | Role |
| --- | --- |
| `activity_main.xml` | `DrawerLayout` that holds the screen and the side menu (`NavigationView`). |
| `app_bar_main.xml` | `CoordinatorLayout` with the black toolbar and a floating action button. |
| `content_main.xml` | Vertical `LinearLayout` with four `ImageButton`s over the `fundo01.png` space background. |

![Home screen design](docs/images/home-screen.svg)

> The drawing above is a reconstruction based on the layout XML and the button images, not a screenshot of the running app.

Design choices:

- **High contrast:** black buttons with light-gray text over a dark space background.
- **Large, simple targets:** one button per section, stacked vertically, with generous spacing.
- **Icon + text:** each button image pairs an icon (book, constellation, planet, storm cloud) with a large label.
- **Spoken labels:** because the text is part of the image, each `ImageButton` has an `android:contentDescription` ("Glossário.", "Carta Celeste.", and so on) that screen readers announce.

---

## Accessibility approach

The project was built for blind and low-vision users, so accessibility is part of the content, not just the interface:

- **Image audio descriptions:** images of the Moon phases, Galilean moons, planets and sky charts carry long `contentDescription` texts describing colors, shapes, craters, bands and positions of objects, so screen-reader users can "see" them through words.
- **Consistent structure:** sections follow the same pattern (menu, then content, then a back button), which keeps navigation predictable.
- **Dedicated screens:** the side menu includes an *Interface description* screen that explains how the app is laid out.
- **Team credits for accessibility work:** the "About" screen credits audio-description and consulting roles alongside design and development.

---

## Project structure

```text
AplicativoAcessibilidade/
├── app/
│   ├── build.gradle
│   └── src/main/
│       ├── AndroidManifest.xml          # ~40 activities, MainActivity is the launcher
│       ├── java/com/example/gabriel/aplicativoacessibilidade/
│       │   ├── MainActivity.java        # home screen + side menu
│       │   ├── Main2Activity.java       # Glossary menu (A-Z)
│       │   ├── Main3Activity.java       # Sky chart menu (months)
│       │   ├── Main4Activity.java       # Astronomical objects menu
│       │   ├── Main5Activity.java       # Weather (WebView)
│       │   ├── ABC.java ... YZ.java     # glossary letter screens
│       │   ├── Carta*.java              # monthly / directional sky charts
│       │   ├── Nebulosas, Constelacoes, Planetas, Lua, SatelitesNaturais, ...
│       │   └── SobreApp, DescricaoInterface, SobreGaturamo, WellcomePage
│       └── res/
│           ├── layout/                  # one XML layout per screen
│           ├── drawable/                # buttons, planets, constellations, backgrounds
│           └── menu/                    # side menu definition
├── build.gradle
└── gradle/
```

---

## Tech stack

- **Language:** Java
- **Platform:** Android (`minSdkVersion 19`, `targetSdkVersion 24`, `compileSdkVersion 26`)
- **UI libraries:** Android Support Library 26 (`appcompat-v7`, `design`, `support-v4`) and ConstraintLayout 1.0.2
- **Build tools:** Android Gradle Plugin 3.0.1 with Gradle 4.1
- **Other:** `WebView` for weather data; Google Play Services Maps 10.2.6 is declared as a dependency

---

## Getting started

1. Clone the repository:

   ```bash
   git clone https://github.com/<your-user>/AplicativoAcessibilidade.git
   ```

2. Open the project in **Android Studio**.
3. Let Gradle sync. Because the project uses an old toolchain (Gradle 4.1, Android Gradle Plugin 3.0.1 and the legacy `android.support.*` libraries), you may need to install the matching SDK platform (API 26) or update the Gradle files to a newer version.
4. Run the app on an emulator or a physical device (Android 4.4 / API 19 or higher).
5. To test accessibility, enable **TalkBack** in the device settings and navigate the app with it.

---

## Known limitations

- The splash screen (`WellcomePage`) exists but is not the launcher activity, so the app opens directly on the home screen. Its text is still a placeholder.
- The floating action button on the home screen comes from the Android Studio template and only shows an example message.
- The side-menu item "Artigos Científicos" has no action yet.
- The weather section depends on an internet connection and on `tempo.ufes.br` being online.
- The home layout uses fixed dimensions, so on larger screens the buttons stay aligned to the left.
- The project uses the deprecated Android Support Library; migrating to AndroidX and a current Gradle version is recommended.

### Possible next steps

- Migrate to AndroidX and update the Gradle toolchain.
- Replace image buttons with text-based buttons (or add real `TextView` labels) for better scaling and localization.
- Add more months and directions to the sky charts.
- Add automated accessibility checks to the build.

---

## Credits

- **Development:** Gabriel B. K. Lopes
- **Project coordination, design, audio descriptions and consulting:** see the *Sobre Aplicativo* screen inside the app
- **Institutional links:** GOA observatory ([Facebook](https://www.facebook.com/goa.observatorio/)) and [astro.ufes.br](http://astro.ufes.br/)

---

## License

No license file is included in this repository yet. Add one (for example MIT or Apache-2.0) before sharing or accepting contributions.
