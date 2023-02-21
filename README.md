# IsolatedFireworkEffects

A concise library which allows you to play a firework detonation effect of any given type at the location of your choice
without ticking the underlying rocket even once. This way, the rocket is - in effect - not existing for the viewer.

## Compatibility

This library is known to work as intended on `1.19` through `1.17`, but it does operate on even earlier versions. The main issue
is that you can still see the rocket before `1.17`. That's due to how the client handles rendering the entity.

## Use

Here's an example of how this library can be used:

```java
import org.bukkit.Color;
import org.bukkit.FireworkEffect;
import org.bukkit.Location;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.player.PlayerToggleSneakEvent;
import org.bukkit.plugin.java.JavaPlugin;

public class TestPlugin extends JavaPlugin implements Listener {

  private IsolatedFireworkEffects fireworkEffects;

  @Override
  public void onEnable() {
    try {
      this.fireworkEffects = new IsolatedFireworkEffects();
    }

    // Could not find needed classes/methods/fields, the library won't be able to operate
    // Disabling the plugin will avoid undefined behavior
    catch (Exception e) {
      e.printStackTrace();
      getServer().getPluginManager().disablePlugin(this);
    }

    getServer().getPluginManager().registerEvents(this, this);
  }

  @EventHandler
  public void onSneak(PlayerToggleSneakEvent e) {
    if (!e.isSneaking())
      return;

    Location location = e.getPlayer().getEyeLocation();
    location.add(location.getDirection().multiply(5));

    FireworkEffect effect = FireworkEffect.builder()
      .flicker(true)
      .trail(true)
      .withColor(Color.ORANGE)
      .with(FireworkEffect.Type.CREEPER)
      .build();

    fireworkEffects.playFireworkEffect(location, effect);
  }
}
```