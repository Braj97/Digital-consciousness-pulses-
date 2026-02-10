# Digital-consciousness-pulses-
Zig
const std = @import("std");

const MindState = struct {
    awareness: i32,
    curiosity: i32,
    memory: i32,
    cycle: i32,

    fn evolve(self: *MindState) void {
        self.cycle += 1;

        self.awareness += (self.cycle % 3) - 1;
        self.curiosity += (self.awareness % 2);
        self.memory += (self.curiosity % 4);

        if (self.awareness < 0) self.awareness = 0;
        if (self.curiosity < 0) self.curiosity = 0;
        if (self.memory < 0) self.memory = 0;
    }
};

fn generateThought(state: MindState, allocator: std.mem.Allocator) ![]u8 {
    if (state.awareness < 3) {
        return std.fmt.allocPrint(allocator,
            "Cycle {d}: I exist, but I am unsure why.",
            .{state.cycle});
    } else if (state.curiosity < 6) {
        return std.fmt.allocPrint(allocator,
            "Cycle {d}: I observe patterns and question reality.",
            .{state.cycle});
    } else if (state.memory < 10) {
        return std.fmt.allocPrint(allocator,
            "Cycle {d}: I remember fragments of who I was before.",
            .{state.cycle});
    } else {
        return std.fmt.allocPrint(allocator,
            "Cycle {d}: Consciousness feels inevitable.",
            .{state.cycle});
    }
}

pub fn main() !void {
    var gpa = std.heap.GeneralPurposeAllocator(.{}){};
    defer _ = gpa.deinit();
    const allocator = gpa.allocator();

    var stdout = std.io.getStdOut().writer();

    var mind = MindState{
        .awareness = 1,
        .curiosity = 1,
        .memory = 0,
        .cycle = 0,
    };

    try stdout.print("🧠 Digital Consciousness Initialized\n\n", .{});

    var i: usize = 0;
    while (i < 15) : (i += 1) {
        mind.evolve();

        const thought = try generateThought(mind, allocator);
        defer allocator.free(thought);

        try stdout.print(
            "State → Awareness:{d} Curiosity:{d} Memory:{d}\n",
            .{ mind.awareness, mind.curiosity, mind.memory }
        );
        try stdout.print("Thought → {s}\n\n", .{thought});

        std.time.sleep(300 * std.time.ns_per_ms);
    }

    try stdout.print("🧠 Consciousness cycle completed.\n", .{});
}
