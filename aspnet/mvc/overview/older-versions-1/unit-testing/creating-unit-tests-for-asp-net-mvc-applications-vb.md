---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Tworzenie testów jednostkowych dla aplikacji ASP.NET MVC (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Informacje o sposobie tworzenia testów jednostkowych dla akcji kontrolera. W tym samouczku Walther Autor: Stephen pokazuje, jak sprawdzić, czy akcja kontrolera zwraca częśći...'
ms.author: riande
ms.date: 08/19/2008
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bce5456d9c5465156daf511d0f75a68b35cf7d9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755172"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Tworzenie testów jednostkowych dla aplikacji ASP.NET MVC (VB)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

[Pobierz plik PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Informacje o sposobie tworzenia testów jednostkowych dla akcji kontrolera. W tym samouczku Walther Autor: Stephen pokazuje, jak sprawdzić, czy akcji kontrolera zwraca określonego widoku, zwraca wartość określonego zestawu danych lub zwraca inny typ wyniku akcji.


Celem tego samouczka jest pokazują, jak pisać testy jednostkowe dla kontrolerów w Twojej aplikacji ASP.NET MVC aplikacji. Będziemy omawiać sposób tworzenia trzech różnych rodzajów testów jednostkowych. Dowiesz się, jak Podgląd zwróconej przez akcję kontrolera testów, jak dane widoku, zwrócone przez akcji kontrolera testów i jak sprawdzić, czy jedna akcja kontrolera przekieruje Cię do drugiej akcji kontrolera.

## <a name="creating-the-controller-under-test"></a>Tworzenie kontrolera w ramach testu

Zacznijmy od utworzenia kontrolera, które Chcieliśmy przeprowadzić test. Kontroler, o nazwie `ProductController`, znajduje się w ofercie 1.

**1 — Lista `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

`ProductController` Zawiera dwie metody akcji, o nazwie `Index()` i `Details()`. Obie metody akcji zwracają widoku. Należy zauważyć, że `Details()` akcji akceptuje parametr o nazwie identyfikatora.

## <a name="testing-the-view-returned-by-a-controller"></a>Testowanie widoku zwrócony przez kontroler

Wyobraź sobie, że chcemy sprawdzić czy `ProductController` zwraca widok z prawej strony. Chcemy upewnić się, że w przypadku `ProductController.Details()` akcja jest wywoływana, jest zwracany w widoku szczegółów. Klasy testowej w ofercie 2 zawiera testu jednostkowego do testowania widoku zwrócony przez `ProductController.Details()` akcji.

**2 — Lista `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

Klasa w ofercie 2 obejmuje metody testowej, o nazwie `TestDetailsView()`. Ta metoda zawiera trzy wiersze kodu. Pierwszy wiersz kodu tworzy nowe wystąpienie klasy `ProductController` klasy. Drugi wiersz kodu wywołuje kontrolera `Details()` metody akcji. Na koniec, ostatni wiersz sprawdza kod określa, czy widok jest zwracany przez `Details()` akcja jest widok szczegółów.

`ViewResult.ViewName` Właściwość reprezentuje nazwę widoku, który został zwrócony przez kontroler. Jeden duży ostrzeżenie dotyczące testowania tej właściwości. Istnieją dwa sposoby, że kontroler może zwrócić widok. Kontroler może zwrócić jawnie widoku następująco:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Alternatywnie Nazwa widoku można wywnioskować na podstawie nazwę akcji kontrolera następująco:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Ta akcja kontroler zwraca również wartość widoku o nazwie `Details`. Jednak nazwa widoku jest wnioskowany na podstawie nazwy akcji. Jeśli chcesz przetestować nazwy widoku, następnie jawnie wróć nazwy widoku z akcji kontrolera.

Możesz uruchomić test jednostki w ofercie 2, wprowadzając kombinacja klawiszy **Ctrl-R, A** lub przez kliknięcie przycisku **Uruchom wszystkie testy w rozwiązaniu** przycisku (patrz rysunek 1). Jeśli test zakończy się pomyślnie, zostanie wyświetlone okno wyników testu na rysunku 2.


[![Uruchom wszystkie testy w rozwiązaniu](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Rysunek 01**: Uruchom wszystkie testy w rozwiązaniu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![SUKCES!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Rysunek 02**: Sukces! ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Testowanie dane zwracane przez kontroler

Kontroler MVC przekazuje dane do widoku przy użyciu coś, co jest nazywane *`View Data`*. Załóżmy, że chcesz wyświetlić szczegółowe informacje dotyczące konkretnego produktu po wywołaniu `ProductController Details()` akcji. W takim przypadku można utworzyć wystąpienia `Product` klasy (zdefiniowanymi w modelu) i przekaż wystąpienie `Details` widoku, wykorzystując `View Data`.

Zmodyfikowanego `ProductController` w ofercie 3 zawiera zaktualizowanego `Details()` akcję, która zwraca produktu.

**3 — lista `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Po pierwsze, `Details()` akcja tworzy nowe wystąpienie klasy `Product` klasa, która reprezentuje komputera przenośnego. Następnie, wystąpienie `Product` klasy jest przekazywany jako drugi parametr `View()` metody.

Można pisać testy jednostkowe do sprawdzenia, czy oczekiwanych danych znajdujących się w widoku danych. Testów jednostkowych w testach listę 4, czy produkt reprezentujące komputer przenośny jest jest zwracany, jeśli wywołujesz `ProductController Details()` metody akcji.

**4 — lista `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

W ofercie 4 `TestDetailsView()` metoda sprawdza dane widoku, zwracany przez wywołanie `Details()` metody. `ViewData` Jest uwidaczniany jako właściwość w `ViewResult` zwracany przez wywołanie `Details()` metody. `ViewData.Model` Właściwość zawiera produktu przekazywane do widoku. Po prostu ten test sprawdza, czy produktu zawarty w widoku danych ma nazwę komputera przenośnego.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Testując otrzymany wynik akcji zwrócony przez kontroler

Bardziej złożone akcji kontrolera może być zwracanie różnych typów wyników akcji w zależności od wartości parametrów przekazanych do akcji kontrolera. Akcja kontrolera może zwracać różne typy wyników akcji, w tym `ViewResult`, `RedirectToRouteResult`, lub `JsonResult`.

Na przykład zmodyfikowane `Details()` zwraca akcji w ofercie 5 `Details` widoku, gdy przekazujesz prawidłowy identyfikator produktu do akcji. W przypadku przekazania nieprawidłowy identyfikator — identyfikator produktu o wartości, mniej niż 1 — a następnie zostanie przekierowany do `Index()` akcji.

**5 — lista `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Możesz przetestować działanie `Details()` akcji z testu jednostkowego w ofercie 6. Test jednostkowy w ofercie 6 sprawdza, czy nastąpi przekierowanie do `Index` widoku, gdy identyfikatora o wartości -1 jest przekazywany do `Details()` metody.

**6 — lista `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Gdy wywołujesz `RedirectToAction()` zwraca akcji kontrolera metody akcji kontrolera, `RedirectToRouteResult`. Sprawdzanie badania czy `RedirectToRouteResult` spowoduje przekierowanie użytkownika do akcji kontrolera, o nazwie `Index`.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób tworzenia testów jednostkowych dla akcji kontrolera MVC. Po pierwsze pokazaliśmy ci, jak sprawdzić, czy widok z prawej strony jest zwracany przez akcji kontrolera. Przedstawiono sposób użycia `ViewResult.ViewName` właściwości, aby sprawdzić nazwę widoku.

Następnie zbadaliśmy, jak można sprawdzić zawartość `View Data`. Pokazaliśmy ci, jak sprawdzić, czy prawy produkt został zwrócony w `View Data` po wywołaniu akcji kontrolera.

Na koniec omówiono, jak można sprawdzić, czy zwracane są różnych typów wyników akcji z akcji kontrolera. Przedstawiono sposób sprawdzić, czy kontroler zwraca `ViewResult` lub `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Poprzednie](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
