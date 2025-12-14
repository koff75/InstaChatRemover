(async function instagramLoopUnlike() {
    // --- CONFIGURATION ---
    // URL : https://www.instagram.com/your_activity/interactions/likes
    const MAX_LOOPS = 10;            // Nombre de répétitions
    const TIME_BETWEEN_LOOPS = 3000; // 3 secondes entre chaque cycle
    const TIME_BETWEEN_CHECKS = 100; // Vitesse de sélection des photos
    const WAIT_FOR_POPUP = 2000;     // Attente du popup

    // Fonction de pause
    const delay = (ms) => new Promise(res => setTimeout(res, ms));

    // Fonction pour trouver un élément par son texte (Méthode robuste)
    function getElementByText(textStrings) {
        const xpath = `//*[${textStrings.map(t => `contains(text(), '${t}')`).join(' or ')}]`;
        const result = document.evaluate(xpath, document, null, XPathResult.ORDERED_NODE_SNAPSHOT_TYPE, null);
        let elements = [];
        for (let i = 0; i < result.snapshotLength; i++) {
            let el = result.snapshotItem(i);
            if (el.tagName === 'SPAN' || el.tagName === 'DIV' || el.tagName === 'BUTTON') {
                 elements.push(el);
            }
        }
        return elements;
    }

    console.log(`🚀 DÉMARRAGE DE L'AUTOMATION (${MAX_LOOPS} tours prévus)`);

    for (let i = 1; i <= MAX_LOOPS; i++) {
        console.log(`\n🔄 --- TOUR ${i} / ${MAX_LOOPS} ---`);

        // --- ÉTAPE 1 : MODE SELECT ---
        // On vérifie si on doit cliquer sur Select
        let selectBtns = getElementByText(["Select", "Sélect."]);
        if (selectBtns.length > 0) {
            // On vérifie si c'est visible et cliquable
            try {
                selectBtns[0].click();
                console.log("1️⃣ Bouton 'Select' cliqué.");
                await delay(1500);
            } catch (e) {
                console.log("ℹ️ Erreur clic Select (peut-être déjà actif), on continue.");
            }
        }

        // --- ÉTAPE 2 : COCHER LES PHOTOS ---
        // On cherche les cases à cocher visibles
        const checkboxes = document.querySelectorAll('div[aria-label="Toggle checkbox"]');

        if (checkboxes.length === 0) {
            console.log("🛑 Plus aucune photo trouvée. Arrêt du script.");
            break; // On sort de la boucle si c'est vide
        }

        console.log(`2️⃣ Sélection de ${checkboxes.length} photos...`);
        for (const box of checkboxes) {
            box.click();
            await delay(TIME_BETWEEN_CHECKS);
        }

        // --- ÉTAPE 3 : CLIC SUR UNLIKE (BARRE DU BAS) ---
        let unlikeTexts = getElementByText(["Unlike", "Je ne plus aimer"]);
        if (unlikeTexts.length > 0) {
            // Le dernier élément visible est celui du bas
            let btnBottom = unlikeTexts[unlikeTexts.length - 1];
            btnBottom.click();
            
            // Sécurité : clic sur le parent si besoin
            if(btnBottom.parentElement) btnBottom.parentElement.click();
            
            console.log("3️⃣ Clic sur 'Unlike' (Barre du bas).");
        } else {
            console.error("❌ Bouton Unlike introuvable. Arrêt.");
            break;
        }

        // --- ÉTAPE 4 : CONFIRMATION POPUP ---
        console.log("⏳ Attente du popup...");
        await delay(WAIT_FOR_POPUP);

        let finalTexts = getElementByText(["Unlike", "Je ne plus aimer", "Supprimer"]);
        if (finalTexts.length > 0) {
            // Le TOUT dernier élément du DOM est forcément celui du popup qui vient d'apparaître
            let confirmBtn = finalTexts[finalTexts.length - 1];
            
            confirmBtn.click();
            // Clics de sécurité sur les parents (spécifique Bloks)
            if(confirmBtn.parentElement) confirmBtn.parentElement.click();
            if(confirmBtn.parentElement && confirmBtn.parentElement.parentElement) confirmBtn.parentElement.parentElement.click();

            console.log("4️⃣ Popup confirmé !");
        } else {
            console.warn("⚠️ Pas de bouton de confirmation trouvé.");
        }

        // --- PAUSE AVANT LE PROCHAIN TOUR ---
        if (i < MAX_LOOPS) {
            console.log(`⏳ Pause de ${TIME_BETWEEN_LOOPS/1000} secondes avant le prochain tour...`);
            await delay(TIME_BETWEEN_LOOPS);
        }
    }

    console.log("\n🎉 TERMINE ! Tous les cycles sont finis.");

})();
