# Version 3.3.7 — Extinction des lumières à l’arrêt de Windows

Les actions **À l’arrêt de Gear Control** s’exécutent aussi lorsque vous choisissez
**Arrêter**, **Redémarrer** ou **Se déconnecter** dans Windows. La règle d’extinction
déjà enregistrée reste utilisable sans modification.

Le lanceur reçoit désormais les notifications natives de fin de session, y compris
lorsque seule son icône près de l’horloge est présente. Il ferme le service avant
les autres processus ordinaires et attend la fin des actions et la déconnexion
des appareils avant de répondre à Windows. La limite globale de dix secondes
pour les automatisations reste en place. Si l’arrêt du PC est annulé, Gear Control
reste actif et aucune action d’arrêt n’est exécutée.

Fermer uniquement la page du hub laisse toujours le service actif. Une coupure de
courant ou un arrêt forcé ne permettent toujours pas de garantir l’extinction.

Vérification native isolée, sans arrêt du PC ni accès au matériel : messages
d’arrêt/redémarrage, de déconnexion et de fermeture par Windows, annulations
répétées, absence de doublon, raison temporaire Windows et priorité d’arrêt.
Une action factice de six secondes se termine avant la réponse à Windows ;
un enfant bloqué est arrêté après son délai de grâce. L’essai complet avec un
véritable arrêt du PC et la lumière réelle reste à effectuer par l’utilisateur.

La suite complète compte **1 016 tests : 1 015 réussis, 1 ignoré, aucun échec**.
Le contrôle Svelte est sans erreur ni avertissement. Le test HTTP du vrai service
en simulateur sur le port 3299 confirme aussi l’extinction relue avant arrêt IPC.

Version construite et installée localement. Le service répond en 3.3.7 avec un
seul lanceur ; sa fenêtre native de fin de session est présente. Les règles
enregistrées et le choix de démarrage avec Windows sont conservés. Aucune
publication effectuée.

Le traitement suit la documentation Microsoft de
[WM_QUERYENDSESSION](https://learn.microsoft.com/en-us/windows/win32/shutdown/wm-queryendsession),
[WM_ENDSESSION](https://learn.microsoft.com/en-us/windows/win32/shutdown/wm-endsession),
[ShutdownBlockReasonCreate](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-shutdownblockreasoncreate)
et de la [priorité d’arrêt des processus](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-setprocessshutdownparameters).
