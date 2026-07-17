    // Override performance.now
    (function() {
    const SPEED = 0.5; // 0.5 = chậm 50%, 0.25 = chậm 75%
    const originalPerfNow = performance.now.bind(performance);
    const startReal = originalPerfNow();

    performance.now = function() {
        const real = originalPerfNow();
        const elapsed = (real - startReal) * SPEED;
        return startReal + elapsed;
    };

    // Override requestAnimationFrame
    const originalRAF = window.requestAnimationFrame;

    window.requestAnimationFrame = function(callback) {
        return originalRAF.call(window, function() {
            callback(performance.now());
        });
    };

    // Override Date.now
    const originalDateNow = Date.now;
    const startDateReal = originalDateNow();

    Date.now = function() {
        const real = originalDateNow();
        const elapsed = (real - startDateReal) * SPEED;
        return startDateReal + elapsed;
    };

    console.log(`✅ Giảm tốc độ xuống ${SPEED}x`);
    })();

